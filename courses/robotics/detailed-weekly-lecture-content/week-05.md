# Week 5: Introduction to ROS 2

## Learning Objectives

By the end of this lecture, you should be able to:

1. Describe the ROS 2 graph of nodes, topics, services and parameters, and say when each is appropriate.
2. Create a workspace and package, and build it with `colcon`.
3. Write a publisher node and a subscriber node with `rclpy`.
4. Inspect a running system with the `ros2` command line tools.
5. Run and test the same publish and subscribe logic without ROS 2, using a small simulation of the middleware, and measure a publish rate.

## 1. ROS 2 Architecture

ROS 2, the Robot Operating System 2, is not an operating system. It is middleware: software that sits between your programs and lets them exchange data. A robot application is built as a graph of many small programs called nodes, each doing one job. One node reads the LiDAR, another builds a map, another plans a path, another drives the motors. This design has two big advantages. Each node can be developed and tested alone, and nodes written by different people, in different languages, can work together.

Nodes talk to each other in three ways.

1. Topics. Many-to-many, asynchronous publish and subscribe. A node publishes messages on a named topic, such as `/scan`, and any number of nodes can subscribe to it. The publisher does not know who is listening, and the subscribers do not know who is publishing. Topics suit continuous streams: sensor readings, velocity commands, estimated poses.
2. Services. Synchronous request and response, like a function call across the network. A client sends a request and waits for a reply. Services suit things that happen once on demand: "compute this value now", "reset the odometry", "save the map".
3. Parameters. Configuration values that a node exposes, such as a maximum speed or a PID gain. They can be given at launch time and changed while the node is running.

A fourth mechanism, actions, is used for long-running goals that give feedback, such as "navigate to this pose". We mention it so that the name is familiar, and we use it later.

Underneath, ROS 2 uses DDS, the Data Distribution Service, to move messages between processes and machines. For this course DDS is an implementation detail, and we do not configure it.

### 1.1 Messages and quality of service

Every topic has a message type, which defines the fields of each message. `std_msgs/String` carries one text field called `data`. `geometry_msgs/Twist` carries a linear velocity and an angular velocity. `sensor_msgs/LaserScan` carries an array of range readings. A publisher and a subscriber must use the same type on the same topic.

The number 10 that you see in `create_publisher(String, 'topic', 10)` is the queue depth, a simple form of quality of service. If a subscriber is slow, up to 10 messages are held for it, and older ones are dropped beyond that. Real systems also choose reliability and durability settings. Sensor streams usually favour the most recent data over guaranteed delivery.

## 2. Workspaces and Packages

Your code lives in a package, and packages live in a workspace.

```bash
mkdir -p ~/ros2_ws/src
cd ~/ros2_ws/src
ros2 pkg create --build-type ament_python my_robot_pkg
cd ~/ros2_ws
colcon build
source install/setup.bash
```

1. A package groups related nodes, launch files and configuration. `ament_python` means a Python package.
2. A workspace is a folder of packages that are built together with `colcon`.
3. After each build you must source `install/setup.bash` in your terminal, so that the new packages can be found. This is separate from sourcing the main ROS 2 installation. Forgetting either is the usual reason for "package not found".

A Python package needs its nodes registered as entry points in `setup.py`, otherwise `ros2 run` will not find them.

```py
# in setup.py, inside setup(...)
entry_points={
    'console_scripts': [
        'minimal_publisher = my_robot_pkg.publisher_node:main',
        'minimal_subscriber = my_robot_pkg.subscriber_node:main',
    ],
},
```

After building, `ros2 run my_robot_pkg minimal_publisher` starts the first node.

## 3. Writing a Publisher Node

This is the standard first node. It publishes a text message every half second.

```py
import rclpy
from rclpy.node import Node
from std_msgs.msg import String

class MinimalPublisher(Node):
    def __init__(self):
        super().__init__('minimal_publisher')
        self.publisher_ = self.create_publisher(String, 'topic', 10)
        self.timer = self.create_timer(0.5, self.timer_callback)
        self.count = 0

    def timer_callback(self):
        msg = String()
        msg.data = f'Hello {self.count}'
        self.publisher_.publish(msg)
        self.get_logger().info(f'Publishing: {msg.data}')
        self.count += 1

def main():
    rclpy.init()
    node = MinimalPublisher()
    try:
        rclpy.spin(node)
    except KeyboardInterrupt:
        pass
    node.destroy_node()
    rclpy.shutdown()

if __name__ == '__main__':
    main()
```

Study its structure, because every node has it.

1. A class derived from `Node`. The call to `super().__init__('minimal_publisher')` gives the node its name.
2. `create_publisher(type, topic, queue_depth)` declares what the node will publish.
3. `create_timer(period, callback)` calls the callback every `period` seconds. This is how a node does periodic work.
4. `rclpy.spin(node)` hands control to ROS 2, which waits for events and calls your callbacks. The program stays inside `spin` until it is stopped.
5. Shutdown cleanly, with `destroy_node` and `rclpy.shutdown`.

## 4. Writing a Subscriber Node

```py
class MinimalSubscriber(Node):
    def __init__(self):
        super().__init__('minimal_subscriber')
        self.subscription = self.create_subscription(
            String, 'topic', self.listener_callback, 10)

    def listener_callback(self, msg):
        self.get_logger().info(f'Received: {msg.data}')
```

The subscriber registers a callback, and ROS 2 calls it each time a message arrives. Note that you never call `listener_callback` yourself. This event-driven style is the heart of ROS programming, and the most common beginner error is to write a loop that polls for data instead of letting the callback do it.

## 5. Running and Testing Without ROS 2

You will do the real thing in the lab. But the logic of publishers, subscribers and timers can be explored anywhere, and an exercise you can run in a minute is the best way to understand the model. The following small module imitates the parts of `rclpy` used above. It runs everything in one process with a virtual clock, so that it needs no installation and is completely deterministic. The names of its functions and methods follow `rclpy`, so the node classes you write for it look almost identical to the real ones.

Treat it as a teaching device and not as a replacement. It has no processes, no network, no queue depth and no real time.

```python
import heapq
import itertools
from dataclasses import dataclass

class _World:
    """Stands in for the ROS 2 middleware: topics, timers and a virtual clock."""
    def __init__(self):
        self.now = 0.0
        self.subs = {}             # topic -> list of callbacks
        self.timers = []           # heap of (fire_time, order, period, callback)
        self.order = itertools.count()
        self.stamps = {}           # topic -> list of publish times

world = _World()

def init():
    global world
    world = _World()

class _Logger:
    def __init__(self, name):
        self.name = name
    def info(self, text):
        print(f"[INFO] [{world.now:6.2f}] [{self.name}]: {text}")

class _Publisher:
    def __init__(self, topic):
        self.topic = topic
    def publish(self, msg):
        world.stamps.setdefault(self.topic, []).append(world.now)
        for callback in list(world.subs.get(self.topic, [])):
            callback(msg)

class Node:
    def __init__(self, name):
        self.name = name
    def get_logger(self):
        return _Logger(self.name)
    def create_publisher(self, msg_type, topic, qos):
        return _Publisher(topic)
    def create_subscription(self, msg_type, topic, callback, qos):
        world.subs.setdefault(topic, []).append(callback)
    def create_timer(self, period, callback):
        heapq.heappush(world.timers, (world.now + period, next(world.order), period, callback))
    def destroy_node(self):
        pass

def spin(seconds):
    """Advance virtual time, firing timers in order (like rclpy.spin, but bounded)."""
    end = world.now + seconds
    while world.timers and world.timers[0][0] <= end + 1e-9:
        t, _, period, callback = heapq.heappop(world.timers)
        world.now = t
        callback()
        heapq.heappush(world.timers, (t + period, next(world.order), period, callback))
    world.now = end

def topic_hz(topic):
    """Average publish rate, like `ros2 topic hz`."""
    times = world.stamps.get(topic, [])
    if len(times) < 2:
        return 0.0
    return (len(times) - 1) / (times[-1] - times[0])

@dataclass
class String:
    data: str = ""
```

The messages here are Python dataclasses, which play the role of `std_msgs/String`. Now the publisher and subscriber, written in the same shape as the real ones.

```python
class MinimalPublisher(Node):
    def __init__(self):
        Node.__init__(self, 'minimal_publisher')
        self.publisher_ = self.create_publisher(String, 'topic', 10)
        self.timer = self.create_timer(0.5, self.timer_callback)
        self.count = 0

    def timer_callback(self):
        msg = String()
        msg.data = f'Hello {self.count}'
        self.publisher_.publish(msg)
        self.get_logger().info(f'Publishing: {msg.data}')
        self.count += 1

class MinimalSubscriber(Node):
    def __init__(self):
        Node.__init__(self, 'minimal_subscriber')
        self.subscription = self.create_subscription(
            String, 'topic', self.listener_callback, 10)
        self.received = []

    def listener_callback(self, msg):
        self.received.append(msg.data)
        self.get_logger().info(f'Received: {msg.data}')

init()
publisher = MinimalPublisher()
subscriber = MinimalSubscriber()
spin(2.0)                      # two seconds of virtual time

print("messages received:", subscriber.received)
print("measured rate: %.2f Hz" % topic_hz('topic'))
```

We write `Node.__init__(self, name)` explicitly in these classes only because our imitation `Node` is not the real one. In real `rclpy` you write `super().__init__('minimal_publisher')`, as in the earlier listing.

You should see four messages, `Hello 0` to `Hello 3`, at times 0.5, 1.0, 1.5 and 2.0 seconds, and a measured rate of 2.00 Hz, which matches the period of 0.5 seconds. A publisher with a period of 0.5 seconds gives a rate of 2 Hz, because the rate is the reciprocal of the period.

## 6. Inspecting with Command Line Tools

When the real system is running, these commands show what is happening. They are used constantly in debugging.

```bash
ros2 topic list           # list active topics
ros2 topic echo /topic    # print messages as they arrive
ros2 topic hz /topic      # measure publish rate
ros2 topic info /topic    # show the type and the number of publishers and subscribers
ros2 node list            # list running nodes
ros2 node info /minimal_publisher
ros2 interface show std_msgs/msg/String
rqt_graph                 # draw the graph of nodes and topics
```

A useful routine whenever something does not work: first check that the node is running with `ros2 node list`, then that its topic exists with `ros2 topic list`, then that messages flow with `ros2 topic echo`, and finally that the rate and type are right with `ros2 topic hz` and `ros2 topic info`. Most connection problems are found in those four steps, and are typically a misspelt topic name or a mismatch of message types.

## 7. Worked Example: Two Rates, One Subscriber

A common situation in robotics is that one subscriber receives data from sources that run at different rates, for example a fast IMU and a slow camera. A subscriber has to cope with this. Here we build two publishers at different rates feeding one node, which keeps the latest value from each and reports a combined status every second.

```python
@dataclass
class Reading:
    source: str = ""
    value: float = 0.0

class FastSensor(Node):
    def __init__(self):
        Node.__init__(self, 'fast_sensor')
        self.pub = self.create_publisher(Reading, 'imu', 10)
        self.t = 0
        self.create_timer(0.1, self.tick)       # 10 Hz
    def tick(self):
        self.t += 1
        self.pub.publish(Reading('imu', 0.01 * self.t))

class SlowSensor(Node):
    def __init__(self):
        Node.__init__(self, 'slow_sensor')
        self.pub = self.create_publisher(Reading, 'camera', 10)
        self.t = 0
        self.create_timer(0.5, self.tick)       # 2 Hz
    def tick(self):
        self.t += 1
        self.pub.publish(Reading('camera', 100.0 + self.t))

class Monitor(Node):
    def __init__(self):
        Node.__init__(self, 'monitor')
        self.latest = {}
        self.count = {'imu': 0, 'camera': 0}
        self.create_subscription(Reading, 'imu', self.on_reading, 10)
        self.create_subscription(Reading, 'camera', self.on_reading, 10)
        self.create_timer(1.0, self.report)
    def on_reading(self, msg):
        self.latest[msg.source] = msg.value
        self.count[msg.source] += 1
    def report(self):
        self.get_logger().info(f"latest {self.latest}  counts {self.count}")

init()
nodes = [FastSensor(), SlowSensor(), Monitor()]
spin(3.0)
print("imu rate: %.1f Hz, camera rate: %.1f Hz" % (topic_hz('imu'), topic_hz('camera')))
```

At each report the monitor sees roughly ten IMU readings and two camera readings per second. The monitor never waits for either sensor, and it never needs to know the other's rate. That is the benefit of publish and subscribe. Later, in the sensor fusion week, we will deal with how to combine such asynchronous streams properly.

## 8. In-Class Exercise

Modify the publisher to send a custom message type at a different rate, and use `ros2 topic hz` to verify that the actual publish rate matches your expectation.

In real ROS 2, a custom message is defined in a `.msg` file in an interface package. For example `RobotStatus.msg`:

```
string name
float32 battery_percent
bool moving
```

Using it needs three edits: add the `.msg` file to the interface package's `CMakeLists.txt` with `rosidl_generate_interfaces`, rebuild with `colcon build`, and import it with `from my_interfaces.msg import RobotStatus`. The node then looks the same as before, with `RobotStatus` in place of `String`.

The following runs the same idea in the simulation. The publisher sends a status message at 4 Hz, and we check the measured rate.

```python
@dataclass
class RobotStatus:
    name: str = ""
    battery_percent: float = 100.0
    moving: bool = False

class StatusPublisher(Node):
    def __init__(self, rate_hz):
        Node.__init__(self, 'status_publisher')
        self.pub = self.create_publisher(RobotStatus, 'status', 10)
        self.battery = 100.0
        self.create_timer(1.0 / rate_hz, self.tick)
    def tick(self):
        self.battery -= 0.5
        self.pub.publish(RobotStatus('rover1', self.battery, moving=True))

class StatusListener(Node):
    def __init__(self):
        Node.__init__(self, 'status_listener')
        self.last = None
        self.create_subscription(RobotStatus, 'status', lambda m: setattr(self, 'last', m), 10)

init()
pub, sub = StatusPublisher(rate_hz=4.0), StatusListener()
spin(10.0)
print("expected 4.0 Hz, measured %.2f Hz" % topic_hz('status'))
print("last message:", sub.last)
```

Questions:

1. The measured rate is exactly the expected rate in the simulation. Why might it be slightly different on a real system?
2. What would you change to publish at 20 Hz? What do you expect `ros2 topic hz` to print?
3. If the subscriber's callback took 0.3 seconds to run, and messages arrived every 0.1 seconds, what would happen? (Think about the queue depth of 10.)

## 9. Common Mistakes

1. Forgetting to source the ROS 2 installation, or the workspace's `install/setup.bash`, in a new terminal.
2. Forgetting to register the node as an entry point in `setup.py`, or forgetting to rebuild after editing it.
3. Using different message types or misspelt names for the publisher and the subscriber, which leads to silence and no error.
4. Writing a `while True` loop inside a node instead of using timers and callbacks. It blocks `spin`, and the node then cannot receive anything.
5. Doing slow work inside a callback, which delays everything else in that node.
6. Forgetting that a service call or other waiting inside a callback can block the node.

## 10. Summary

ROS 2 organizes a robot program as a graph of nodes that exchange typed messages through topics, make on-demand requests through services, and are configured through parameters. A node is a class derived from `Node`, which creates publishers, subscriptions and timers, and then lets `spin` run the callbacks. The `ros2` command line tools make the running graph visible, and the simulation in this lecture lets you test the logic of nodes and rates on any machine. Next week we add services, parameters, launch files and a simulated robot.

## 11. Practice Problems

1. Write a node that subscribes to `imu` readings and publishes a smoothed value on a new topic, using a moving average of the last five readings. Test it in the simulation.
2. Write two publishers on the same topic at different rates. What does the subscriber see? What does `topic_hz` report?
3. In the real ROS 2, publish `geometry_msgs/Twist` messages on `/cmd_vel` from a node, and use `ros2 topic echo` to check them.
4. Draw the node and topic graph for the monitor example, in the style of `rqt_graph`.

## 12. Suggested Reading

1. The official ROS 2 tutorials, "Beginner: CLI tools" and "Beginner: Client libraries".
2. Macenski et al., "Robot Operating System 2: Design, architecture, and uses in the wild", Science Robotics, 2022.
