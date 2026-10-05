# Week 6: ROS 2 Continued, Services, Parameters and Simulation

## Learning Objectives

By the end of this lecture, you should be able to:

1. Choose between a topic and a service for a given communication need.
2. Write a service server and a client, and call a service from the command line.
3. Declare node parameters, read them, and change them at launch time and at run time.
4. Write a launch file that starts several nodes with their parameters.
5. Drive a simulated differential-drive robot by publishing velocity commands, and check its response.

## 1. Services versus Topics

Last week we used topics, which are continuous one-way streams. A service is a different tool. It is a one-off request and reply, like calling a function in another program.

1. Use a topic for a stream of data that many nodes may want, and whose publisher does not care who is listening. For example, "publish the battery level every 100 milliseconds".
2. Use a service when a node needs an answer to a specific question, or wants something done once, and needs to know that it has been done. For example, "tell me the battery level right now", or "reset the odometry".

Three points are useful when deciding.

1. A topic message goes to everyone who subscribes. A service reply goes only to the caller.
2. A service call has a result. A topic publication does not tell the sender whether anybody received it.
3. A service should be quick. If the work takes seconds and gives progress updates, use an action instead. Never use a service for anything that must run continuously.

A service has a type with two parts, a request and a response. The standard `example_interfaces/srv/AddTwoInts` has the request fields `a` and `b`, and the response field `sum`.

```py
import rclpy
from rclpy.node import Node
from example_interfaces.srv import AddTwoInts

class AddTwoIntsServer(Node):
    def __init__(self):
        super().__init__('add_two_ints_server')
        self.srv = self.create_service(AddTwoInts, 'add_two_ints', self.add_callback)

    def add_callback(self, request, response):
        response.sum = request.a + request.b
        self.get_logger().info(f'{request.a} + {request.b} = {response.sum}')
        return response

def main():
    rclpy.init()
    node = AddTwoIntsServer()
    rclpy.spin(node)
    rclpy.shutdown()
```

The server registers a callback, which fills in the response object and returns it. A client written in `rclpy` creates a client, waits for the service to become available, and sends a request asynchronously.

```py
class AddTwoIntsClient(Node):
    def __init__(self):
        super().__init__('add_two_ints_client')
        self.client = self.create_client(AddTwoInts, 'add_two_ints')
        while not self.client.wait_for_service(timeout_sec=1.0):
            self.get_logger().info('service not available, waiting...')

    def send(self, a, b):
        request = AddTwoInts.Request()
        request.a, request.b = a, b
        future = self.client.call_async(request)
        rclpy.spin_until_future_complete(self, future)
        return future.result().sum
```

From a terminal you can also call a service without writing a client.

```bash
ros2 service list
ros2 service type /add_two_ints
ros2 service call /add_two_ints example_interfaces/srv/AddTwoInts "{a: 3, b: 4}"
```

Calling `call_async` and waiting on a future, instead of a plain blocking call, is deliberate. A blocking call inside a callback can freeze the node, so ROS 2 encourages the asynchronous style.

## 2. Parameters

A parameter is a named value that belongs to a node. It is the right place for settings that you may want to change without editing code: a speed limit, a controller gain, a topic name, a map file.

```py
class ConfigurableNode(Node):
    def __init__(self):
        super().__init__('configurable_node')
        self.declare_parameter('max_speed', 1.0)
        max_speed = self.get_parameter('max_speed').value
        self.get_logger().info(f'max_speed = {max_speed}')
```

Declaring a parameter, with a default value, is required in ROS 2. The declaration also tells the system the parameter's type. Parameters can be set when the node starts, or changed while it runs.

```bash
ros2 run my_robot_pkg configurable_node --ros-args -p max_speed:=0.5
ros2 param list
ros2 param get /configurable_node max_speed
ros2 param set /configurable_node max_speed 2.0
```

If your node should respond when a parameter changes, which is common for controller gains, it registers a callback with `add_on_set_parameters_callback`. Without it, a node that reads a parameter only once in `__init__` will never notice the change.

## 3. Launch Files

A robot system is many nodes with many parameters. Starting each in its own terminal is slow and error prone. A launch file starts them together.

```py
from launch import LaunchDescription
from launch_ros.actions import Node

def generate_launch_description():
    return LaunchDescription([
        Node(package='my_robot_pkg', executable='minimal_publisher', name='publisher'),
        Node(package='my_robot_pkg', executable='minimal_subscriber', name='subscriber',
             parameters=[{'max_speed': 0.5}]),
    ])
```

Start it with `ros2 launch my_robot_pkg my_launch.py`. A launch file is itself a Python program that returns a description of what to start, which means that you can use conditions and loops in it. In a package, the launch folder must be installed by `setup.py` through the `data_files` entry, otherwise `ros2 launch` cannot find it.

## 4. Bringing Up a Simulated Robot

The simulators Gazebo and Webots connect to ROS 2 through bridge nodes. The bridge makes the simulated robot look like a real one: its sensors appear as topics, for example `/scan` for the laser and `/camera/image_raw` for the camera, and it accepts velocity commands on `/cmd_vel`. The robot's pose estimate normally appears on `/odom`. The exact bring-up command depends on the simulation package supplied by your instructor, and is usually a single `ros2 launch` command.

Once it is running, the same commands as before tell you what is available.

```bash
ros2 topic list
ros2 topic echo /odom --once
ros2 topic info /cmd_vel
```

Before we use the real simulator in the lab, we shall try the same ideas on a tiny stand-in that you can run on any computer.

## 5. Running and Testing Without ROS 2

The module below extends the one from Week 5 with services, parameters and a small launch helper. As before it runs in a single process, on a virtual clock, so results are deterministic. It follows the `rclpy` names, but it is a teaching aid and not the real thing.

```python
import heapq
import itertools
import math
from dataclasses import dataclass, field

class _World:
    def __init__(self):
        self.now = 0.0
        self.subs = {}
        self.services = {}
        self.timers = []
        self.order = itertools.count()
        self.stamps = {}

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

class _Param:
    def __init__(self, value):
        self.value = value

class _Client:
    def __init__(self, srv_type, name):
        self.srv_type, self.name = srv_type, name
    def call(self, request):
        srv_type, callback = world.services[self.name]
        return callback(request, srv_type.Response())

class Node:
    def __init__(self, name, parameters=None):
        self.name = name
        self._params = {}
        self._overrides = dict(parameters or {})
    def get_logger(self):
        return _Logger(self.name)
    def create_publisher(self, msg_type, topic, qos):
        return _Publisher(topic)
    def create_subscription(self, msg_type, topic, callback, qos):
        world.subs.setdefault(topic, []).append(callback)
    def create_timer(self, period, callback):
        heapq.heappush(world.timers, (world.now + period, next(world.order), period, callback))
    def create_service(self, srv_type, name, callback):
        world.services[name] = (srv_type, callback)
    def create_client(self, srv_type, name):
        return _Client(srv_type, name)
    def declare_parameter(self, name, default):
        self._params[name] = self._overrides.get(name, default)
    def get_parameter(self, name):
        return _Param(self._params[name])
    def set_parameter(self, name, value):
        self._params[name] = value

def spin(seconds):
    end = world.now + seconds
    while world.timers and world.timers[0][0] <= end + 1e-9:
        t, _, period, callback = heapq.heappop(world.timers)
        world.now = t
        callback()
        heapq.heappush(world.timers, (t + period, next(world.order), period, callback))
    world.now = end

def launch(*entries):
    """Like a launch file: each entry is (NodeClass, parameter dict)."""
    return [cls(parameters=params) if params else cls() for cls, params in entries]

# message and service types, as plain dataclasses
@dataclass
class Vector3:
    x: float = 0.0
    y: float = 0.0
    z: float = 0.0

@dataclass
class Twist:
    linear: Vector3 = field(default_factory=Vector3)
    angular: Vector3 = field(default_factory=Vector3)

@dataclass
class Odometry:
    x: float = 0.0
    y: float = 0.0
    theta: float = 0.0
```

### 5.1 A service

```python
class AddTwoInts:
    @dataclass
    class Request:
        a: int = 0
        b: int = 0
    @dataclass
    class Response:
        sum: int = 0

class AddTwoIntsServer(Node):
    def __init__(self):
        Node.__init__(self, 'add_two_ints_server')
        self.srv = self.create_service(AddTwoInts, 'add_two_ints', self.add_callback)

    def add_callback(self, request, response):
        response.sum = request.a + request.b
        self.get_logger().info(f'{request.a} + {request.b} = {response.sum}')
        return response

init()
server = AddTwoIntsServer()
client_node = Node('client')
client = client_node.create_client(AddTwoInts, 'add_two_ints')

reply = client.call(AddTwoInts.Request(a=3, b=4))
print("reply:", reply)
reply = client.call(AddTwoInts.Request(a=-10, b=2))
print("reply:", reply)
```

The server only does work when called, and nothing is published. If you were to publish the sum on a topic instead, every node would receive every sum, whether or not it asked.

### 5.2 Parameters and a launch helper

```python
class SpeedLimiter(Node):
    def __init__(self, parameters=None):
        Node.__init__(self, 'speed_limiter', parameters)
        self.declare_parameter('max_speed', 1.0)
        self.max_speed = self.get_parameter('max_speed').value
        self.get_logger().info(f'max_speed = {self.max_speed}')

    def limit(self, v):
        return max(-self.max_speed, min(self.max_speed, v))

init()
default_node, = launch((SpeedLimiter, None))
tuned_node, = launch((SpeedLimiter, {'max_speed': 0.3}))

for v in [-1.0, 0.1, 0.5, 2.0]:
    print(f"requested {v:5.1f}   default limiter {default_node.limit(v):5.2f}   tuned limiter {tuned_node.limit(v):5.2f}")

tuned_node.set_parameter('max_speed', 0.8)
print("after set_parameter, the stored value is", tuned_node.get_parameter('max_speed').value)
print("but the node still limits to", tuned_node.max_speed, "because it copied the value in __init__")
```

The last two lines show the trap mentioned earlier. The node copied the value when it started, so changing the parameter has no effect on the behaviour. A node that must respond to changes should read the parameter each time, or react in a parameter callback. We fix this next.

```python
class LiveSpeedLimiter(SpeedLimiter):
    def limit(self, v):
        m = self.get_parameter('max_speed').value       # read on every call
        return max(-m, min(m, v))

init()
live, = launch((LiveSpeedLimiter, {'max_speed': 0.3}))
print(live.limit(2.0))
live.set_parameter('max_speed', 0.8)
print(live.limit(2.0))
```

The first call prints 0.3 and the second 0.8, so the node responds to the change.

## 6. Driving the Simulated Robot

To control a robot, you publish velocity commands. The message type is `geometry_msgs/Twist`, which has a linear velocity vector and an angular velocity vector. For a ground robot only two fields matter: `linear.x`, the forward speed in metres per second, and `angular.z`, the turning rate in radians per second.

```py
from geometry_msgs.msg import Twist

class DriveForward(Node):
    def __init__(self):
        super().__init__('drive_forward')
        self.publisher_ = self.create_publisher(Twist, '/cmd_vel', 10)
        self.timer = self.create_timer(0.1, self.timer_callback)

    def timer_callback(self):
        msg = Twist()
        msg.linear.x = 0.2    # m/s forward
        msg.angular.z = 0.0   # rad/s turning
        self.publisher_.publish(msg)
```

Why keep publishing every 0.1 seconds? Many robot drivers stop the robot if they do not hear a command for a short time. This is a safety feature, sometimes called a watchdog. If your controller crashes, the robot comes to rest instead of driving on.

Here is the same controller in our test bench, together with a simple simulated robot. The robot node listens for `/cmd_vel`, integrates the differential-drive model from Week 3 every 0.05 seconds, and publishes its pose on `/odom`. It also stops if no command has arrived for 0.5 seconds.

```python
class SimulatedRobot(Node):
    def __init__(self):
        Node.__init__(self, 'sim_robot')
        self.x = self.y = self.theta = 0.0
        self.v = self.w = 0.0
        self.last_cmd_time = -1e9
        self.odom_pub = self.create_publisher(Odometry, '/odom', 10)
        self.create_subscription(Twist, '/cmd_vel', self.on_cmd, 10)
        self.dt = 0.05
        self.create_timer(self.dt, self.step)

    def on_cmd(self, msg):
        self.v, self.w = msg.linear.x, msg.angular.z
        self.last_cmd_time = world.now

    def step(self):
        if world.now - self.last_cmd_time > 0.5:        # watchdog
            self.v = self.w = 0.0
        self.x += self.v * math.cos(self.theta) * self.dt
        self.y += self.v * math.sin(self.theta) * self.dt
        self.theta += self.w * self.dt
        self.odom_pub.publish(Odometry(self.x, self.y, self.theta))

class DriveForward(Node):
    def __init__(self, speed=0.2, turn=0.0, stop_after=None):
        Node.__init__(self, 'drive_forward')
        self.publisher_ = self.create_publisher(Twist, '/cmd_vel', 10)
        self.speed, self.turn, self.stop_after = speed, turn, stop_after
        self.timer = self.create_timer(0.1, self.timer_callback)

    def timer_callback(self):
        if self.stop_after is not None and world.now > self.stop_after:
            return                                       # stop commanding, as if the node crashed
        msg = Twist()
        msg.linear.x = self.speed
        msg.angular.z = self.turn
        self.publisher_.publish(msg)

class OdomListener(Node):
    def __init__(self):
        Node.__init__(self, 'odom_listener')
        self.last = None
        self.create_subscription(Odometry, '/odom', lambda m: setattr(self, 'last', m), 10)

init()
robot, driver, listener = SimulatedRobot(), DriveForward(speed=0.2), OdomListener()
spin(5.0)
print("after 5 s at 0.2 m/s the robot is at x = %.2f m, y = %.2f m" % (listener.last.x, listener.last.y))
```

The result should be about 1.0 metre forward, since 0.2 times 5 seconds is 1.0, and the sideways position is zero. The printed value is 0.99, a little under 1.0, because the first command only arrives after the first 0.1 second timer tick, and the robot only reacts to it a moment later. This kind of start-up delay is typical, and you should expect it from real systems too.

Next, a gentle curve, then the watchdog. At a turn rate of 0.5 radians per second, six seconds turn the robot by 3 radians, about 172 degrees, so it ends up heading back roughly towards where it started.

```python
init()
robot, driver, listener = SimulatedRobot(), DriveForward(speed=0.3, turn=0.5), OdomListener()
spin(6.0)
o = listener.last
print("curving drive:  x = %.2f  y = %.2f  heading = %.1f deg" % (o.x, o.y, math.degrees(o.theta)))

init()
robot, driver, listener = SimulatedRobot(), DriveForward(speed=0.3, stop_after=2.0), OdomListener()
spin(5.0)
o = listener.last
print("controller 'crashes' at t = 2 s, robot stopped at x = %.2f  (without a watchdog it would be at 1.50)" % o.x)
```

With the watchdog, the robot stops soon after the commands cease, at about 0.7 metres, rather than driving on to 1.5. This is a useful property to remember when designing your own controllers.

## 7. In-Class Exercise

Write a simple service that returns whether a given `(x, y)` coordinate lies inside the robot's declared workspace bounds, with the bounds passed in as parameters.

In real ROS 2, the service type is defined in a `CheckPoint.srv` file in an interface package. The two parts are separated by three dashes.

```
float64 x
float64 y
---
bool inside
```

The server reads four parameters and compares. In `rclpy` it looks like this.

```py
class WorkspaceServer(Node):
    def __init__(self):
        super().__init__('workspace_server')
        for name, default in [('x_min', -1.0), ('x_max', 1.0), ('y_min', -1.0), ('y_max', 1.0)]:
            self.declare_parameter(name, default)
        self.srv = self.create_service(CheckPoint, 'check_point', self.callback)

    def callback(self, request, response):
        p = lambda n: self.get_parameter(n).value
        response.inside = (p('x_min') <= request.x <= p('x_max')
                           and p('y_min') <= request.y <= p('y_max'))
        return response
```

And the version that runs on the test bench.

```python
class CheckPoint:
    @dataclass
    class Request:
        x: float = 0.0
        y: float = 0.0
    @dataclass
    class Response:
        inside: bool = False

class WorkspaceServer(Node):
    def __init__(self, parameters=None):
        Node.__init__(self, 'workspace_server', parameters)
        for name, default in [('x_min', -1.0), ('x_max', 1.0), ('y_min', -1.0), ('y_max', 1.0)]:
            self.declare_parameter(name, default)
        self.create_service(CheckPoint, 'check_point', self.callback)

    def callback(self, request, response):
        p = lambda n: self.get_parameter(n).value
        response.inside = (p('x_min') <= request.x <= p('x_max')
                           and p('y_min') <= request.y <= p('y_max'))
        return response

init()
server, = launch((WorkspaceServer, {'x_max': 2.0, 'y_min': 0.0}))
caller = Node('caller').create_client(CheckPoint, 'check_point')

tests = [(0.0, 0.5), (1.5, 0.5), (2.5, 0.5), (0.0, -0.5), (-1.0, 0.0), (2.0, 1.0)]
for x, y in tests:
    print((x, y), "inside" if caller.call(CheckPoint.Request(x, y)).inside else "outside")
```

With the bounds `-1 <= x <= 2` and `0 <= y <= 1`, the first, second, fifth and last points are inside, and the third and fourth are outside. Notice that the boundary points `(-1, 0)` and `(2, 1)` count as inside, because we used `<=`.

Questions:

1. Should the test use `<` or `<=` at the boundary? Which is safer for a robot, and why?
2. How would you make the server react if someone changes `x_max` while it is running? (It already does here. Why?)
3. What could go wrong if a client calls the service with a `nan` coordinate? Try it, and decide what the server should do.

## 8. Common Mistakes

1. Using a service for a continuous stream, or a topic for something that needs a confirmed answer.
2. Calling a service in a blocking way from inside a callback.
3. Reading a parameter once in `__init__` and expecting later changes to take effect.
4. Forgetting `declare_parameter`, which makes `get_parameter` fail in ROS 2.
5. Publishing `/cmd_vel` once and expecting the robot to keep going. Publish repeatedly.
6. Publishing a velocity larger than the robot can safely do, with no limit anywhere in the chain.
7. Forgetting to install the launch folder in `setup.py`.

## 9. Summary

Services give a reliable request and response between nodes, and topics give continuous streams. Parameters let us configure a node from outside, but only help if the node actually re-reads them. Launch files start whole systems with a single command. A simulated robot is driven by publishing `Twist` messages on `/cmd_vel` and observed through `/odom`, and a watchdog that stops the robot when commands stop is an important safety habit. Next week we look at the physics that explains why a robot moves the way it does.

## 10. Practice Problems

1. Add a service `reset_odometry` to `SimulatedRobot` that sets its pose to zero. Call it in the middle of a run and check that `/odom` restarts from zero.
2. Write a node that subscribes to `/odom` and publishes a `/cmd_vel` that makes the robot stop when it is 1 metre from the start. Test it with the simulated robot.
3. Make `DriveForward` take its speed and turn rate from parameters, and launch two instances with different settings.
4. Write the launch file for the real ROS 2 version of the workspace example, with the parameters set to the values used in the test.

## 11. Suggested Reading

1. The official ROS 2 tutorials on services, parameters and launch files.
2. The documentation of the simulation package that your instructor provides.
