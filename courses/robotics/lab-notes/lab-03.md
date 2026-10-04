# Lab Notes 3 — Forward Kinematics

**Concept recap:** forward kinematics chains per-joint rotations/link lengths to compute
end-effector position; the differential-drive model integrates linear/angular velocity over
time to update `(x, y, theta)`.

**Common pitfalls:**
- Angle unit confusion (degrees vs. radians) — NumPy's trig functions expect radians; convert
  explicitly if test cases are given in degrees.
- Accumulating pose updates with too large a `dt`, producing a visibly jagged/inaccurate
  trajectory compared to smaller time steps.

**Debugging tip:** for Task A, test joint angles of `0` and `pi/2` first — these have simple,
easy-to-predict-by-hand expected outputs, and catch sign/axis errors quickly.

**Instructor tip:** Task C's two trajectories (straight line vs. circle) give an intuitive,
visual check that the differential-drive model is implemented correctly — a bug often shows up
as a trajectory that curves unexpectedly or drifts off in the wrong direction.
