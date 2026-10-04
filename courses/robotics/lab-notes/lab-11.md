# Lab Notes 11 — OpenCV Object Detection

**Concept recap:** HSV thresholding + contour detection is the standard simple pipeline for
color-based object detection; `cv_bridge` connects ROS 2 `Image` messages to OpenCV's NumPy image
representation.

**Common pitfalls:**
- OpenCV loads images in BGR order by default, not RGB — forgetting this causes color-channel
  confusion when picking threshold bounds or displaying with `matplotlib` (which expects RGB).
- Picking HSV bounds too narrow, missing the object entirely under slightly different lighting.
- Not handling the "no contours found" case, causing a crash when `max()` is called on an empty
  list.

**Debugging tip:** display the binary mask (`cv2.imshow`/`plt.imshow(mask, cmap='gray')`)
directly — it immediately shows whether the threshold is too narrow, too wide, or picking up the
wrong region.

**Instructor tip:** Task D's robustness check is meant to be a little frustrating — the goal is
for students to directly experience that pure color thresholding is not lighting-invariant,
motivating why real systems often use more robust features or learned detectors.
