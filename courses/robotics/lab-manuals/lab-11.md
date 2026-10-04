# Lab Manual 11 — OpenCV Object Detection

**Duration:** 3 hours | **Prerequisite:** Week 11 lecture

## Objectives
Build an OpenCV color-detection pipeline and integrate it with ROS 2.

## Setup
Create `lab11_pkg`; use the instructor-provided simulated camera feed in Gazebo/Webots.

## Procedure
1. **Task A — HSV thresholding:** write code to convert a captured frame to HSV and threshold
   for a given target color.
2. **Task B — Contour detection:** find contours in the mask and compute the bounding box of the
   largest detected region.
3. **Task C — ROS 2 integration:** write a node using `cv_bridge` that subscribes to the
   simulated camera topic, runs the detection pipeline, and publishes the bounding box
   coordinates on a custom/simple topic.
4. **Task D — Robustness check:** test detection under 2 different simulated lighting conditions;
   report the HSV threshold adjustments needed (if any).

## Expected Output
A working package with Tasks A–D; a short report on the robustness check results.

## Submission
Submit the package + report by the end of the lab session.
