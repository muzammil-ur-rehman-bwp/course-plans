# Week 6 — Lecture Content: ROS 2 Continued — Services, Parameters, Simulation

## 1. Services vs. Topics
A service is for a one-off request/response (e.g., "give me the current battery level right
now"), while a topic is for a continuous stream (e.g., "publish battery level every 100ms").
```python
from example_interfaces.srv import AddTwoInts

class AddTwoIntsServer(Node):
    def __init__(self):
        super().__init__('add_two_ints_server')
        self.srv = self.create_service(AddTwoInts, 'add_two_ints', self.add_callback)

    def add_callback(self, request, response):
        response.sum = request.a + request.b
        return response
```

## 2. Parameters
```python
class ConfigurableNode(Node):
    def __init__(self):
        super().__init__('configurable_node')
        self.declare_parameter('max_speed', 1.0)
        max_speed = self.get_parameter('max_speed').value
```
Parameters can be set at launch time or changed at runtime via `ros2 param set`.

## 3. Launch Files
```python
from launch import LaunchDescription
from launch_ros.actions import Node

def generate_launch_description():
    return LaunchDescription([
        Node(package='my_robot_pkg', executable='minimal_publisher', name='publisher'),
        Node(package='my_robot_pkg', executable='minimal_subscriber', name='subscriber'),
    ])
```
Launch files start multiple nodes (and set their parameters) with a single command
(`ros2 launch my_robot_pkg my_launch.py`), avoiding manual multi-terminal setup.

## 4. Bringing Up a Simulated Robot
Gazebo/Webots integrate with ROS 2 via bridge nodes that expose the simulated robot's sensors as
topics (e.g., `/scan`, `/camera/image_raw`) and accept velocity commands on `/cmd_vel`. The exact
bring-up command depends on the provided simulation package (instructor-supplied launch file).

## 5. Driving the Simulated Robot
```python
from geometry_msgs.msg import Twist

class DriveForward(Node):
    def __init__(self):
        super().__init__('drive_forward')
        self.publisher_ = self.create_publisher(Twist, '/cmd_vel', 10)
        self.timer = self.create_timer(0.1, self.timer_callback)

    def timer_callback(self):
        msg = Twist()
        msg.linear.x = 0.2   # m/s forward
        msg.angular.z = 0.0  # rad/s turning
        self.publisher_.publish(msg)
```

## 6. In-Class Exercise
Write a simple service that returns whether a given `(x, y)` coordinate lies within the robot's
declared workspace bounds (bounds passed in as parameters).
