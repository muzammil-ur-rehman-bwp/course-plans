# Week 11 Summary — Computer Vision for Robotics

**Key takeaways:**
- A camera projects the 3D world onto a 2D image array; OpenCV provides the tools to manipulate
  and analyze these arrays.
- HSV color space separates color from brightness, making color-based detection more robust to
  lighting than raw RGB/BGR.
- The threshold → find contours → bounding box pipeline is the standard simple approach to
  detecting a known-colored object.
- `cv_bridge` connects ROS 2 `Image` messages to OpenCV's NumPy-based image representation,
  letting a vision node subscribe to a camera topic and publish detection results.

**You should now be able to:** build a basic OpenCV color-detection pipeline; integrate it with
ROS 2 via `cv_bridge`.

**Reminder:** Assignment 2 (control + vision) assigned this week, due start of Week 13.
**Next week:** state estimation and sensor fusion via the Kalman filter.
