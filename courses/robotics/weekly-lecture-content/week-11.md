# Week 11 — Lecture Content: Computer Vision for Robotics

## 1. The Pinhole Camera Model (Conceptual)
A camera projects 3D points in the world onto a 2D image plane through a single point (the
"pinhole"). We treat this at a conceptual level in this course: closer/larger objects project to
larger regions of the image; the camera has a field of view beyond which points aren't visible.
An image is represented as a 2D (grayscale) or 3D (color, H x W x 3) array of pixel intensities.

## 2. OpenCV Fundamentals
```python
import cv2

img = cv2.imread('frame.png')               # BGR by default in OpenCV
gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
hsv = cv2.cvtColor(img, cv2.COLOR_BGR2HSV)
blurred = cv2.GaussianBlur(img, (5, 5), 0)
cv2.imshow('frame', img)   # or plt.imshow for notebooks (convert BGR->RGB first)
```
**HSV** (Hue, Saturation, Value) separates color (Hue) from brightness, making color-based
detection far more robust to lighting changes than working directly in RGB/BGR.

## 3. Color-Based Object Detection
```python
import numpy as np

lower_red = np.array([0, 120, 70])
upper_red = np.array([10, 255, 255])
mask = cv2.inRange(hsv, lower_red, upper_red)
contours, _ = cv2.findContours(mask, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)
largest = max(contours, key=cv2.contourArea) if contours else None
if largest is not None:
    x, y, w, h = cv2.boundingRect(largest)
```
This pipeline (threshold → find contours → bounding box) is the standard, simple pattern for
detecting a known-colored object in a scene.

## 4. ROS 2 Integration
```python
from cv_bridge import CvBridge
from sensor_msgs.msg import Image

class VisionNode(Node):
    def __init__(self):
        super().__init__('vision_node')
        self.bridge = CvBridge()
        self.subscription = self.create_subscription(
            Image, '/camera/image_raw', self.image_callback, 10)

    def image_callback(self, msg):
        cv_image = self.bridge.imgmsg_to_cv2(msg, desired_encoding='bgr8')
        # run the detection pipeline on cv_image, then publish results
```
`cv_bridge` converts between ROS 2 `Image` messages and OpenCV's NumPy array representation.

## 5. In-Class Exercise
Tune the HSV threshold bounds to reliably detect a given colored object under two different
simulated lighting conditions, and discuss why HSV thresholding alone is not fully lighting-
invariant.
