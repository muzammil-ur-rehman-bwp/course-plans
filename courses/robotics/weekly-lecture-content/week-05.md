# Week 5 — Lecture Content: Introduction to ROS 2

## 1. ROS 2 Architecture
ROS 2 (Robot Operating System 2) is middleware for building robot software as a graph of
**nodes** that communicate via:
- **Topics**: many-to-many, asynchronous publish/subscribe (e.g., a sensor continuously
  publishing readings).
- **Services**: synchronous request/response (e.g., "recompute this value once, now").
- **Parameters**: configuration values a node can expose and that can be changed at runtime.

Underneath, ROS 2 uses **DDS** (Data Distribution Service) for communication — we treat this as
an implementation detail, not something to configure directly in this course.

## 2. Workspaces & Packages
```bash
mkdir -p ~/ros2_ws/src
cd ~/ros2_ws/src
ros2 pkg create --build-type ament_python my_robot_pkg
cd ~/ros2_ws
colcon build
source install/setup.bash
```
A **package** groups related nodes, launch files, and configuration. A **workspace** is a
collection of packages built together with `colcon`.

## 3. Writing a Publisher Node (`rclpy`)
```python
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
        self.count += 1

def main():
    rclpy.init()
    node = MinimalPublisher()
    rclpy.spin(node)
    node.destroy_node()
    rclpy.shutdown()
```

## 4. Writing a Subscriber Node (`rclpy`)
```python
class MinimalSubscriber(Node):
    def __init__(self):
        super().__init__('minimal_subscriber')
        self.subscription = self.create_subscription(
            String, 'topic', self.listener_callback, 10)

    def listener_callback(self, msg):
        self.get_logger().info(f'Received: {msg.data}')
```

## 5. Inspecting with CLI Tools
```bash
ros2 topic list          # list active topics
ros2 topic echo /topic   # print messages as they arrive
ros2 topic hz /topic     # measure publish rate
ros2 node list           # list running nodes
```

## 6. In-Class Exercise
Modify the publisher to send a custom message type at a different rate, and use `ros2 topic hz`
to verify the actual publish rate matches expectations.
