# Week 9 — Lecture Content: Midterm + Sensors II — Range Sensing

## 1. Midterm Exam
Covers Weeks 1–8 (transforms, kinematics, ROS 2, dynamics/actuators, odometry). See Week 8's
review materials for practice problems.

## 2. Range Sensors
- **LiDAR**: emits laser pulses and measures time-of-flight (or phase shift) to compute distance
  to the nearest obstacle at each angle, producing a 2D (or 3D) scan.
- **Ultrasonic**: emits sound pulses, measures echo time — cheaper, shorter range, wider beam
  (less precise angularly) than LiDAR.
- **IR (Infrared) range sensors**: measure distance via reflected infrared light intensity or
  triangulation — short range, sensitive to surface reflectivity/color.

ROS 2 represents a 2D LiDAR scan with `sensor_msgs/LaserScan`: an array of range readings at
evenly spaced angles between `angle_min` and `angle_max`.

## 3. From Range Scan to Occupancy Grid
An occupancy grid discretizes the environment into cells, each holding a probability/flag of
being occupied.
```python
import numpy as np

def scan_to_occupancy(ranges, angle_min, angle_increment, grid_size, resolution, robot_pos):
    grid = np.zeros((grid_size, grid_size))
    for i, r in enumerate(ranges):
        if r == float('inf') or np.isnan(r):
            continue
        angle = angle_min + i * angle_increment
        x = robot_pos[0] + r * np.cos(angle)
        y = robot_pos[1] + r * np.sin(angle)
        gx, gy = int(x / resolution), int(y / resolution)
        if 0 <= gx < grid_size and 0 <= gy < grid_size:
            grid[gy, gx] = 1  # mark occupied
    return grid
```
This directly reuses the frame-transform skills from Week 2: each range+angle reading is a point
in the sensor's local frame that must be converted to the world/grid frame using the robot's
current pose.

## 4. In-Class Exercise
Given a simulated LiDAR scan (array of ranges + angles) and a known robot pose, build the
occupancy grid using the function above, and visualize it as a 2D image.
