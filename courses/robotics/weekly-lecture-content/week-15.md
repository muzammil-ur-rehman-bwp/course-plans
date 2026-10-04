# Week 15 — Lecture Content: Autonomous Navigation Pipeline

## 1. Integrating the Pieces
```
LiDAR/Camera --> Occupancy Grid (Wk 9) --> Path Planner (Wk 13/14, A*/RRT)
      |                                           |
      v                                           v
Encoders/IMU --> Kalman Filter (Wk 12) --> Current Pose Estimate --> PID Controller (Wk 10) --> /cmd_vel
```
Every arrow in this diagram is a piece students have already built by hand in a prior lab —
Week 15 is about wiring them together into one running ROS 2 system, not learning new theory.

## 2. The ROS 2 Navigation Stack (Nav2) — Overview
Nav2 is the production-grade ROS 2 package that performs localization, mapping, global/local
path planning, and control, all configurable and already optimized/tested at scale. We study it
here only as a conceptual reference point: "this is what the hand-built pipeline is a simplified
version of" — using Nav2 directly is not required for the capstone, though ambitious students
may explore it as an extension.

## 3. Assembling a Minimal Pipeline
1. **Sense**: subscribe to `/scan` (LiDAR) and `/odom` (odometry).
2. **Estimate**: fuse odometry with any available correction signal via the Week 12 Kalman
   filter pattern (or use raw odometry if no correction source is used).
3. **Plan**: build/update the occupancy grid; run A* (or RRT) from current position to the goal.
4. **Control**: feed the next waypoint on the planned path as the PID controller's setpoint;
   publish the resulting command to `/cmd_vel`.
5. **Repeat**: re-plan periodically or when new obstacles are detected (simple replanning loop,
   not full dynamic replanning theory).

## 4. Minimal Working Example (Structure Only)
```python
class NavigationNode(Node):
    def __init__(self):
        super().__init__('navigation_node')
        self.scan_sub = self.create_subscription(LaserScan, '/scan', self.scan_cb, 10)
        self.odom_sub = self.create_subscription(Odometry, '/odom', self.odom_cb, 10)
        self.cmd_pub = self.create_publisher(Twist, '/cmd_vel', 10)
        self.timer = self.create_timer(0.2, self.control_loop)
        self.path = None

    def scan_cb(self, msg):
        self.occupancy_grid = scan_to_occupancy(msg.ranges, ...)  # from Week 9

    def odom_cb(self, msg):
        self.current_pose = (msg.pose.pose.position.x, msg.pose.pose.position.y)

    def control_loop(self):
        if self.path is None:
            self.path = a_star_grid(self.occupancy_grid, self.current_pose, self.goal)
        # compute PID output toward next waypoint on self.path, publish Twist
```

## 5. In-Class Exercise
Assemble and run the minimal pipeline in simulation: drive the robot from a start to a goal
position while avoiding at least one simulated obstacle.
