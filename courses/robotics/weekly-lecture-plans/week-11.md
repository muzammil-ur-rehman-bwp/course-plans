# Week 11 Lecture Plan — Robotics
## Topic: Computer Vision for Robotics

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Explain the pinhole camera model and basic image representation. (*Understand*)
2. Apply OpenCV to detect colored objects/features in a camera feed. (*Apply*)
3. Analyze detection results to assess robustness under varying conditions. (*Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:25 | Camera model | Pinhole model (conceptual), image as a pixel array |
| 0:25–0:55 | OpenCV fundamentals | Read/display/filter images, color spaces (RGB/HSV) |
| 0:55–1:05 | Break | — |
| 1:05–1:35 | Color-based detection | HSV thresholding to detect a colored object, live demo |
| 1:35–2:00 | ROS 2 integration | Publishing detection results as a topic, `cv_bridge` |

### Materials/Equipment
- OpenCV (`opencv-python`), ROS 2 (`sensor_msgs/Image`, `cv_bridge`)
- Simulated camera feed (Gazebo/Webots)

### Formative Check (in-class)
Exercise: tune HSV thresholds to reliably detect a given colored object under two different
simulated lighting conditions.

### Link to Lab/Assessment
Lab 11: build an OpenCV detection pipeline and publish results as a ROS 2 topic.
**Assignment 2 assigned this week** (control + vision), due start of Week 13.
