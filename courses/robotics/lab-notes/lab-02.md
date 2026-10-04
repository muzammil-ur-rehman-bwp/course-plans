# Lab Notes 2 — Coordinate Frames & Transformations

**Concept recap:** a homogeneous transform bundles rotation + translation into one matrix;
composing transforms is matrix multiplication (`T_world_robot @ T_robot_sensor`); inverting a
transform (Task D) can use `np.linalg.inv`, though a closed-form inverse for a rigid transform
(transpose the rotation block, adjust translation) is more numerically stable in practice.

**Common pitfalls:**
- Forgetting the homogeneous coordinate (appending `1` to 2D points before transforming).
- Multiplying transforms in the wrong order — matrix multiplication is not commutative, and
  `T_world_robot @ T_robot_sensor` is not the same as `T_robot_sensor @ T_world_robot`.

**Debugging tip:** test every transform function against a simple case you can verify by hand
(e.g., a 90-degree rotation of a point on an axis) before trusting it on more complex inputs.

**Instructor tip:** this lab's `transform_point`/composition functions are reused directly in
Lab 9 (LiDAR scan to occupancy grid) — make sure every student leaves with working, tested code.
