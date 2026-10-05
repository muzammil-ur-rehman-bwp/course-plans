# Week 11: Computer Vision for Robotics

## Learning Objectives

By the end of this lecture, you should be able to:

1. Explain the pinhole camera model, and use it to relate an object's size in the image to its distance.
2. Represent images as arrays, and convert between BGR, grayscale and HSV.
3. Detect a coloured object with thresholding, contours and a bounding box.
4. Make the detector more robust to lighting changes and noise, and say honestly where it still fails.
5. Turn a detection into a steering command, and connect the pipeline to ROS 2 with `cv_bridge`.

## 1. The Pinhole Camera Model

A camera projects the three dimensional world onto a flat two dimensional sensor. The simplest model of this is the pinhole camera. All light rays pass through a single point, the pinhole, and land on the image plane behind it. The consequences are intuitive.

1. Objects that are further away appear smaller.
2. Straight lines in the world remain straight in the image.
3. The camera has a field of view, and anything outside it is not seen.
4. A single image loses depth. A point in the image corresponds to a whole ray of points in the world.

In numbers, a point at `(X, Y, Z)` in the camera frame, where `Z` is the distance along the optical axis, lands at the pixel

```
u = fx * X / Z + cx
v = fy * Y / Z + cy
```

where `fx` and `fy` are the focal lengths in pixels, and `(cx, cy)` is the pixel where the optical axis meets the image, usually near the centre. We treat the camera at this conceptual level and do not discuss lens distortion or calibration.

An image is stored as an array. A grayscale image is a 2D array of brightness values. A colour image is a 3D array of shape height by width by 3, one value for each colour channel. Row index first, then column. Values are usually integers from 0 to 255.

```python
import numpy as np

def project(point_cam, fx=500.0, fy=500.0, cx=320.0, cy=180.0):
    X, Y, Z = point_cam
    return fx * X / Z + cx, fy * Y / Z + cy

for Z in [0.5, 1.0, 2.0, 4.0]:
    u, v = project((0.2, 0.0, Z))          # a point 20 cm to the right of the optical axis
    print(f"distance {Z:3.1f} m: the point lands at pixel column {u:6.1f}")
```

The same point, 20 centimetres off the axis, lands at column 520 when it is half a metre away, but only at column 345 when it is four metres away. It moves toward the centre of the image as it gets further.

### 1.1 Estimating distance from size

The model gives a very useful trick. If we know the real width `W` of an object, its width in the image is `w = fx * W / Z`, so the distance is

```
Z = fx * W / w
```

```python
def distance_from_width(pixel_width, real_width, fx=500.0):
    return fx * real_width / pixel_width

ball_diameter = 0.08                                     # an 8 cm ball
for w_px in [200, 100, 50, 25]:
    print(f"ball appears {w_px:3d} px wide  ->  distance {distance_from_width(w_px, ball_diameter):.2f} m")
```

This works with one camera, but only when the real size is known and the camera's focal length in pixels has been measured.

## 2. OpenCV Fundamentals

OpenCV is the standard library for image processing. It represents images as NumPy arrays, so everything we learned about arrays applies. For these exercises we avoid depending on image files, by drawing our own test scene with OpenCV's drawing functions. This also gives us a scene whose true contents we know exactly, and so a way to test our detector.

```python
import cv2

def make_scene(brightness=1.0, noise=0.0, seed=0, ball_x=400, ball_r=40):
    rng = np.random.default_rng(seed)
    img = np.full((360, 640, 3), (170, 160, 150), np.uint8)       # a grey wall (colours are B, G, R)
    cv2.rectangle(img, (0, 250), (639, 359), (120, 130, 125), -1)  # the floor
    cv2.circle(img, (ball_x, 230), ball_r, (30, 30, 200), -1)      # red ball
    cv2.circle(img, (150, 240), 30, (200, 60, 40), -1)             # blue ball
    cv2.circle(img, (530, 220), 25, (40, 180, 40), -1)             # green ball
    img = img.astype(float) * brightness + rng.normal(0, noise * 255, img.shape)
    return np.clip(img, 0, 255).astype(np.uint8)

img = make_scene()
print("image shape:", img.shape, " dtype:", img.dtype)
print("pixel at the centre of the red ball (B, G, R):", img[230, 400])
```

The image has 360 rows and 640 columns. A common source of confusion is that OpenCV stores the channels in the order blue, green, red, not red, green, blue. The ball that we drew as `(30, 30, 200)` is therefore strongly red.

```python
gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
hsv = cv2.cvtColor(img, cv2.COLOR_BGR2HSV)
blurred = cv2.GaussianBlur(img, (5, 5), 0)

print("gray shape:", gray.shape)
print("HSV of the red ball:  ", hsv[230, 400])
print("HSV of the blue ball: ", hsv[240, 150])
print("HSV of the green ball:", hsv[220, 530])
```

To display an image in a notebook with Matplotlib, convert it to RGB first, because Matplotlib expects red first.

```python
import matplotlib.pyplot as plt

plt.imshow(cv2.cvtColor(img, cv2.COLOR_BGR2RGB))
plt.title("Test scene")
plt.axis("off")
plt.show()
```

`cv2.imshow` opens its own window and needs a display, so it is not available on a remote or headless machine. In ROS 2 programs, we usually publish the processed image on a topic and look at it with `rqt_image_view`.

### 2.1 Why HSV?

In BGR, the colour and the brightness are mixed up in all three numbers. The same red ball in shadow has quite different BGR values from the red ball in sunlight. HSV separates them: Hue is the colour itself, as an angle around the colour wheel (OpenCV stores it as 0 to 179), Saturation is how pure or washed out the colour is, and Value is the brightness. Selecting by hue and saturation, and being lenient on value, makes detection far more robust to changes of lighting than working in BGR.

```python
for b in [1.0, 0.6, 0.35]:
    dark = make_scene(brightness=b)
    print(f"brightness {b:4.2f}: BGR {dark[230, 400]}   HSV {cv2.cvtColor(dark, cv2.COLOR_BGR2HSV)[230, 400]}")
```

The BGR values change in two channels, while in HSV only the third number, the value, changes much. The hue stays at 0, and the saturation hardly changes.

## 3. Colour-Based Object Detection

The standard simple pipeline has four steps: convert to HSV, threshold to a binary mask, find the contours of the white regions, and take the largest one.

```python
lower_red = np.array([0, 120, 70])
upper_red = np.array([10, 255, 255])

def detect_red(img):
    hsv = cv2.cvtColor(img, cv2.COLOR_BGR2HSV)
    mask = cv2.inRange(hsv, lower_red, upper_red)
    contours, _ = cv2.findContours(mask, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)
    largest = max(contours, key=cv2.contourArea) if contours else None
    if largest is None:
        return None
    x, y, w, h = cv2.boundingRect(largest)
    return x, y, w, h

box = detect_red(make_scene())
print("bounding box (x, y, w, h):", box)
x, y, w, h = box
print("centre of the box: (%d, %d)  true centre of the ball: (400, 230)" % (x + w // 2, y + h // 2))
```

The box is 81 by 81 pixels, the diameter of the ball plus the pixel at the edge, and its centre is at the true centre of the ball. The threshold function `inRange` returns 255 where all three channels lie within the bounds, and 0 elsewhere. `findContours` returns the outlines of connected white regions.

Note a detail: `findContours` returns two values in current OpenCV versions. Some older tutorials show three. If you see an unpacking error, check your OpenCV version.

Combine the detector with the distance estimate of section 1.1.

```python
for r in [40, 20, 10]:
    box = detect_red(make_scene(ball_r=r))
    w_px = box[2]
    print(f"ball radius {r:2d} px: width {w_px:3d} px, distance estimate {distance_from_width(w_px, 0.08):.2f} m")
```

Halving the radius doubles the estimated distance, as it should.

## 4. Making It Robust

The detector above works under good lighting, on a clean image. Real images are not like that, so here we test its limits. We darken the scene, then add noise.

```python
print("brightness  detected?  box")
for b in [1.0, 0.6, 0.35, 0.15]:
    print(f"{b:10.2f}  ", end="")
    box = detect_red(make_scene(brightness=b))
    print("yes       " if box else "NO        ", box)
```

At the lowest brightness the detector sees nothing. The ball's value channel has fallen to 30, which is below our lower bound of 70, so those pixels are rejected. The hue is unchanged, but the threshold on value cuts it out. Loosening the lower bound of the value channel helps, but at some point very dark pixels become meaningless, because their hue is dominated by noise. A lower bound that suits the application must be chosen with care.

### 4.1 Hue wraps around

Red is awkward in HSV. Hue is an angle, so red sits both at the very beginning of the scale, near 0, and at the very end, near 179. A slightly orange red has a hue of 5, a slightly purple red has a hue of 175. A range of 0 to 10 captures only half of the reds. Noise and lighting shift the hue by a few units, and some pixels of the same ball fall on the other side.

```python
noisy = make_scene(noise=0.08, seed=3)
hsv = cv2.cvtColor(noisy, cv2.COLOR_BGR2HSV)
ball_hues = hsv[215:245, 385:415, 0].ravel()
print("hue values on the ball: min", ball_hues.min(), " max", ball_hues.max())
print("fraction with hue above 170:", round(float((ball_hues > 170).mean()), 2))
print("fraction with hue 0 to 10:  ", round(float((ball_hues <= 10).mean()), 2))
```

A sizeable part of the ball's pixels have a hue near 175 to 179, and a one-sided range misses them. The fix is to use two ranges and combine the masks.

```python
def red_mask(hsv, s_min=120, v_min=50):
    low = cv2.inRange(hsv, np.array([0, s_min, v_min]), np.array([10, 255, 255]))
    high = cv2.inRange(hsv, np.array([170, s_min, v_min]), np.array([179, 255, 255]))
    return cv2.bitwise_or(low, high)

hsv = cv2.cvtColor(noisy, cv2.COLOR_BGR2HSV)
one_range = cv2.inRange(hsv, lower_red, upper_red)
two_ranges = red_mask(hsv)
print("mask pixels, one hue range:  ", int((one_range > 0).sum()))
print("mask pixels, two hue ranges: ", int((two_ranges > 0).sum()))
print("area of a perfect disc of radius 40: ", int(np.pi * 40 ** 2))
```

The two-range mask finds nearly the whole disc, about 5,070 pixels against the 5,026 of a perfect disc, while the single range finds only 2,780, just over half. We also lowered the value bound from 70 to 50, to tolerate more shadow.

### 4.2 Noise and clean-up

Noise puts isolated false pixels in the mask. A morphological opening, which erodes the mask to remove small specks and then dilates it to restore the shape of the large regions, removes them. A closing does the opposite and fills small holes.

```python
kernel = np.ones((5, 5), np.uint8)

def detect_red_robust(img, min_area=200):
    hsv = cv2.cvtColor(cv2.GaussianBlur(img, (5, 5), 0), cv2.COLOR_BGR2HSV)
    mask = red_mask(hsv)
    mask = cv2.morphologyEx(mask, cv2.MORPH_OPEN, kernel)
    mask = cv2.morphologyEx(mask, cv2.MORPH_CLOSE, kernel)
    contours, _ = cv2.findContours(mask, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)
    contours = [c for c in contours if cv2.contourArea(c) >= min_area]
    if not contours:
        return None
    c = max(contours, key=cv2.contourArea)
    x, y, w, h = cv2.boundingRect(c)
    return x, y, w, h

print("brightness  noise   plain detector      robust detector")
for b, n in [(1.0, 0.0), (0.35, 0.0), (0.15, 0.0), (1.0, 0.15), (0.35, 0.15)]:
    scene = make_scene(brightness=b, noise=n, seed=5)
    print(f"{b:10.2f}  {n:5.2f}   {str(detect_red(scene)):20s} {detect_red_robust(scene)}")
```

Three additions did the work here: a blur before thresholding, the combination of two hue ranges, and removal of small blobs. In the table, find the rows where the plain detector fails or returns a wrong box, and compare the box with the true one, `(360, 190, 81, 81)`.

### 4.3 What HSV does not fix

HSV makes colour detection more robust to brightness, but it is not lighting-invariant. Coloured light changes the hue. A warm lamp shifts everything towards red and orange, and a white wall can pass the red test. Very bright, saturated highlights lose their saturation, and very dark shadows lose their hue. Cameras also adjust their exposure and white balance automatically, which changes the colours from frame to frame. Other red objects in the scene look the same as the target. For serious applications robots use more robust methods, such as learned detectors, but a thresholding pipeline is a good first step and an excellent teaching tool.

```python
warm = make_scene()
warm = np.clip(warm.astype(float) * np.array([0.6, 0.9, 1.3]), 0, 255).astype(np.uint8)   # B, G, R gains: a warm lamp
hsv_warm = cv2.cvtColor(warm, cv2.COLOR_BGR2HSV)
print("hue of the grey wall under warm light:", hsv_warm[50, 50], " (was", cv2.cvtColor(make_scene(), cv2.COLOR_BGR2HSV)[50, 50], ")")
print("red mask pixels under warm light:", int((red_mask(hsv_warm) > 0).sum()))
```

Under the warm lamp, the grey wall changes from a hue of 105, a bluish grey, to a hue of 14, which is orange, and its saturation rises from 30 to 122. That is only just outside the red range of 0 to 10. A slightly stronger lamp, or a wall that was warmer to begin with, would put it inside the range, and the detector would report a wall as a ball. The lesson is that thresholds must be tuned for the lighting that the robot will actually meet.

## 5. From Detection to Action

A detection is only useful if the robot acts on it. The simplest use is to steer toward the object. The horizontal offset of the object from the image centre is the error, and a proportional controller, as in Week 10, turns it into an angular velocity.

```python
IMAGE_WIDTH = 640

def steering_command(box, k=0.8, max_turn=1.0):
    """Angular velocity (rad/s) to bring the object to the centre of the image. Positive is a left turn."""
    if box is None:
        return 0.0, "no target: stop (or search)"
    x, y, w, h = box
    error = (x + w / 2 - IMAGE_WIDTH / 2) / (IMAGE_WIDTH / 2)      # -1 (far left) to +1 (far right)
    omega = float(np.clip(-k * error, -max_turn, max_turn))        # an object on the right needs a right (negative) turn
    return omega, f"error {error:+.2f}"

for ball_x in [100, 320, 560]:
    box = detect_red_robust(make_scene(ball_x=ball_x))
    omega, note = steering_command(box)
    print(f"ball at column {ball_x:3d}: {note},  angular.z = {omega:+.2f} rad/s")
print("no ball:", steering_command(None))
```

The sign deserves attention. In ROS the convention is that a positive angular velocity turns the robot to the left, anticlockwise. An object on the right of the image needs a right turn, so the command is negative. Getting this sign wrong makes a robot that turns away from its target, and is a classic first-lab bug.

The size of the box gives the distance, and the distance can set the forward speed. The robot then approaches the object, and stops 0.4 metres away.

```python
def approach_command(box, stop_distance=0.4, k_forward=0.5, max_speed=0.3, real_width=0.08):
    if box is None:
        return 0.0
    distance = distance_from_width(box[2], real_width)
    return float(np.clip(k_forward * (distance - stop_distance), 0.0, max_speed))

for r in [10, 20, 40, 60]:
    box = detect_red_robust(make_scene(ball_r=r))
    print(f"ball radius {r:2d} px: distance {distance_from_width(box[2], 0.08):.2f} m  forward speed {approach_command(box):.2f} m/s")
```

## 6. ROS 2 Integration

In ROS 2, the camera driver publishes `sensor_msgs/Image` messages on a topic such as `/camera/image_raw`. The package `cv_bridge` converts them to and from the NumPy arrays that OpenCV uses. The node below subscribes to the image, runs the pipeline, and publishes a velocity command. It requires ROS 2 and a camera (real or simulated), so it cannot run in a plain Python session, but its shape is exactly that of the earlier examples.

```py
import rclpy
from rclpy.node import Node
from cv_bridge import CvBridge
from sensor_msgs.msg import Image
from geometry_msgs.msg import Twist

class VisionNode(Node):
    def __init__(self):
        super().__init__('vision_node')
        self.bridge = CvBridge()
        self.subscription = self.create_subscription(
            Image, '/camera/image_raw', self.image_callback, 10)
        self.cmd_pub = self.create_publisher(Twist, '/cmd_vel', 10)

    def image_callback(self, msg):
        cv_image = self.bridge.imgmsg_to_cv2(msg, desired_encoding='bgr8')
        box = detect_red_robust(cv_image)          # the functions defined above
        omega, _ = steering_command(box)
        cmd = Twist()
        cmd.angular.z = omega
        cmd.linear.x = approach_command(box)
        self.cmd_pub.publish(cmd)

def main():
    rclpy.init()
    rclpy.spin(VisionNode())
    rclpy.shutdown()
```

The argument `desired_encoding='bgr8'` asks `cv_bridge` for the OpenCV channel order, whatever the camera used. If the camera works at 30 frames per second, the callback runs 30 times a second, so it has to be fast. A slow image callback causes a growing delay between what the camera saw and what the robot does. Keep heavy processing out of it, or run it at a lower rate.

## 7. In-Class Exercise

Tune the HSV threshold bounds to detect a given coloured object reliably under two different simulated lighting conditions, and discuss why HSV thresholding alone is not fully lighting-invariant.

Try this with the blue ball. Find bounds that detect it under both normal and dim light, and under a warm lamp.

```python
def detect_color(img, lower, upper, min_area=200):
    hsv = cv2.cvtColor(cv2.GaussianBlur(img, (5, 5), 0), cv2.COLOR_BGR2HSV)
    mask = cv2.morphologyEx(cv2.inRange(hsv, np.array(lower), np.array(upper)), cv2.MORPH_OPEN, kernel)
    contours, _ = cv2.findContours(mask, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)
    contours = [c for c in contours if cv2.contourArea(c) >= min_area]
    return cv2.boundingRect(max(contours, key=cv2.contourArea)) if contours else None

normal = make_scene()
dim = make_scene(brightness=0.3)
warm = np.clip(make_scene().astype(float) * np.array([0.6, 0.9, 1.3]), 0, 255).astype(np.uint8)

print("blue ball HSV, normal:", cv2.cvtColor(normal, cv2.COLOR_BGR2HSV)[240, 150])
print("blue ball HSV, dim:   ", cv2.cvtColor(dim, cv2.COLOR_BGR2HSV)[240, 150])
print("blue ball HSV, warm:  ", cv2.cvtColor(warm, cv2.COLOR_BGR2HSV)[240, 150])

for name, scene in [("normal", normal), ("dim", dim), ("warm", warm)]:
    narrow = detect_color(scene, [100, 150, 100], [130, 255, 255])
    wide = detect_color(scene, [95, 100, 30], [135, 255, 255])
    print(f"{name:7s} narrow bounds: {narrow}   wide bounds: {wide}")
```

Study which bounds work in which conditions. The blue ball really spans 61 pixels starting at `(120, 210)`. The detector's box, `(121, 211, 59, 59)`, is a pixel smaller on each side because of the blur and the opening. In the dim scene the value of the ball falls to about 60, so the narrow bounds, which require a value of at least 100, miss it completely, and in the warm scene the saturation drops and the hue moves. Only the wide bounds find the ball in all three. Then write down a short explanation of the limits of what you found.

Questions:

1. Why are wider bounds not simply always better?
2. If two objects of the same colour appear, which one does `max(contours, key=cv2.contourArea)` choose? Is that what you want?
3. How would you detect a red ball that is partly hidden behind another object?

## 8. Common Mistakes

1. Treating OpenCV images as RGB when they are BGR.
2. Forgetting that hue wraps, for red in particular.
3. Mixing up `(row, column)` and `(x, y)`: `img[y, x]` but `cv2.circle(img, (x, y), ...)`.
4. Using `cv2.imshow` on a machine with no display.
5. Applying fixed thresholds without checking the lighting.
6. Doing slow work inside an image callback.
7. Steering with the wrong sign.
8. Estimating the distance without knowing the object size or the focal length.

## 9. Summary

A camera projects the world through a pinhole, which makes size in the image depend on distance, and this lets us estimate distance if the real size is known. OpenCV treats images as arrays, usually in BGR order. HSV makes colour detection much more tolerant of brightness changes, and with two hue ranges for red, a blur, and morphological clean-up, a simple threshold, contour and bounding box pipeline works quite well in a controlled environment. It is still sensitive to the colour of the light, so thresholds must be tuned and tested. A detection becomes behaviour when it is turned into velocity commands, and in ROS 2 `cv_bridge` connects the camera topic to the same code.

## 10. Practice Problems

1. Extend the detector to find the largest of three colours (red, green, blue) and return its name and box.
2. Compute the circularity `4 pi area / perimeter^2` of each contour, and use it to reject non-round blobs.
3. Add a second box to the scene, a red rectangle, and use circularity to tell it from the ball.
4. Write a function that estimates the bearing angle of the object, from the pixel column and `fx`, using `atan2(u - cx, fx)`. Compare it with the P steering command.

## 11. Suggested Reading

1. The OpenCV Python tutorials on colour spaces, thresholding and contours.
2. Corke, Robotics, Vision and Control, the chapters on image formation and image processing.
