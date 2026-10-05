# Week 4: Inverse Kinematics

## Learning Objectives

By the end of this lecture, you should be able to:

1. State the inverse kinematics (IK) problem, and explain why it can have two solutions, one, or none.
2. Derive and implement the analytical IK of a 2-link planar arm with the law of cosines.
3. Check any IK answer by running it back through forward kinematics.
4. Explain how Jacobian-based iterative IK works, and implement a simple version.
5. Plot a workspace, and use it to judge in advance whether a target is reachable.

## 1. The Inverse Kinematics Problem

Last week we answered a direct question: given the joint angles, where is the end-effector? That is forward kinematics, and it has exactly one answer. A robot user asks the reverse question. "Put the gripper at this point." Which joint angles achieve that? This is inverse kinematics.

It is a harder problem, for three reasons.

1. There may be several solutions. A 2-link arm can often reach the same point with the elbow bent up or bent down.
2. There may be no solution. If the target is beyond the arm's stretch, or too close to the base for a given pair of links, no joint angles reach it.
3. There may be infinitely many solutions. An arm with more joints than the task needs, called a redundant arm, can reach a point in a continuum of ways.

Because of these, an IK routine has to do more than output angles. It must tell the caller when no solution exists, and it must have a rule for choosing among solutions.

## 2. Analytical IK for a 2-Link Planar Arm

For two links in a plane there is a neat closed form, which comes from the geometry of a triangle. The base, the elbow and the target form a triangle with sides `l1`, `l2` and `d`, where `d = sqrt(x^2 + y^2)` is the distance from the base to the target.

### 2.1 Reachability

A triangle can only be formed if each side is no longer than the sum of the other two, and no shorter than their difference. For our arm this gives the condition:

```
|l1 - l2|  <=  d  <=  l1 + l2
```

If `d > l1 + l2`, the target is further than the fully stretched arm can reach. If `d < |l1 - l2|`, the target is too close to the base: with unequal links the shorter one cannot fold back far enough. The reachable region is an annulus, a ring, centred on the base. If the links are equal, the inner radius is zero and the region is a full disc.

### 2.2 Finding the angles

By the law of cosines on the triangle:

```
d^2 = l1^2 + l2^2 - 2 l1 l2 cos(pi - theta2)
```

which gives

```
cos(theta2) = (d^2 - l1^2 - l2^2) / (2 l1 l2)
```

Taking the inverse cosine gives two values, a positive one and its negative. These are the two elbow configurations. Then `theta1` is the direction to the target, minus the angle by which the first link must be rotated off that direction.

```
theta1 = atan2(y, x) - atan2(l2 sin(theta2), l1 + l2 cos(theta2))
```

The code below returns both solutions, and it returns an empty list when the target cannot be reached.

```python
import numpy as np

def forward_kinematics_2link(theta1, theta2, l1, l2):
    x1, y1 = l1 * np.cos(theta1), l1 * np.sin(theta1)
    x2 = x1 + l2 * np.cos(theta1 + theta2)
    y2 = y1 + l2 * np.sin(theta1 + theta2)
    return x2, y2

def inverse_kinematics_2link(x, y, l1, l2):
    """Return a list of (theta1, theta2) solutions: elbow-down first, then elbow-up.
    The list is empty when the target is unreachable."""
    d = np.sqrt(x**2 + y**2)
    if d > l1 + l2 or d < abs(l1 - l2):
        return []                                    # target unreachable

    cos_theta2 = (d**2 - l1**2 - l2**2) / (2 * l1 * l2)
    theta2 = np.arccos(np.clip(cos_theta2, -1.0, 1.0))

    solutions = []
    for t2 in (theta2, -theta2):                     # elbow-down (+), elbow-up (-)
        k1 = l1 + l2 * np.cos(t2)
        k2 = l2 * np.sin(t2)
        t1 = np.arctan2(y, x) - np.arctan2(k2, k1)
        solutions.append((t1, t2))
    if np.isclose(theta2, 0.0) or np.isclose(theta2, np.pi):   # fully stretched or folded flat: one solution
        solutions = solutions[:1]
    return solutions

l1, l2 = 1.0, 0.8
for t1, t2 in inverse_kinematics_2link(1.0, 1.2, l1, l2):
    fx, fy = forward_kinematics_2link(t1, t2, l1, l2)
    print(f"theta1 = {np.rad2deg(t1):7.2f}  theta2 = {np.rad2deg(t2):7.2f}   forward check -> ({fx:.4f}, {fy:.4f})")
```

The `np.clip` call protects against rounding that pushes the cosine slightly outside the range from minus one to one when the target is at the edge of the workspace. Note that we reserve the term "elbow-down" for the solution with a positive `theta2`, in which the elbow lies below the line from the base to the target. Always state your convention, since other books differ.

### 2.3 A hand calculation

For `l1 = 1`, `l2 = 0.8` and the target `(1.0, 1.2)`:

1. `d^2 = 1 + 1.44 = 2.44`, so `d = 1.562`. It lies between `|1 - 0.8| = 0.2` and `1.8`, so the target is reachable.
2. `cos(theta2) = (2.44 - 1 - 0.64) / (2 * 1 * 0.8) = 0.8 / 1.6 = 0.5`, so `theta2 = 60` degrees or `-60` degrees.
3. For `theta2 = 60`: `k1 = 1 + 0.8 * 0.5 = 1.4`, `k2 = 0.8 * 0.8660 = 0.6928`. Then `theta1 = atan2(1.2, 1.0) - atan2(0.6928, 1.4) = 50.19 - 26.34 = 23.85` degrees.
4. For `theta2 = -60`: `k2 = -0.6928`, so `theta1 = 50.19 + 26.34 = 76.53` degrees.

The code output should show the pairs `(23.85, 60)` and `(76.53, -60)`, and both forward checks should return `(1.0000, 1.2000)`.

### 2.4 Unreachable targets

Using the `d` calculation, we can say why a target fails.

```python
for target in [(1.7, 0.7), (1.9, 0.0), (0.1, 0.1), (0.2, 0.0), (1.8, 0.0)]:
    d = np.hypot(*target)
    sols = inverse_kinematics_2link(*target, l1, l2)
    if not sols:
        why = "too far" if d > l1 + l2 else "too close to the base"
        print(f"target {target}: d = {d:.3f}  UNREACHABLE ({why})")
    else:
        print(f"target {target}: d = {d:.3f}  {len(sols)} solution(s)")
```

With these links the reachable annulus runs from `d = 0.2` to `d = 1.8`. The point `(1.9, 0)` is too far. The point `(0.1, 0.1)` has `d = 0.141`, which is inside the inner radius, so it is too close. The two boundary cases at `d = 0.2` and `d = 1.8` are reachable with exactly one solution, since the arm is then folded flat or fully stretched. At those points the two elbow solutions merge.

## 3. Numerical, Iterative IK

The closed form works for a 2-link arm, but for arms with more joints, or awkward geometry, closed forms get complicated or do not exist. Iterative methods then take over. The idea is to start from a guess and improve it step by step, which is conceptually similar to the gradient descent you meet in machine learning.

The key object is the Jacobian. It is the matrix `J` that says how a small change in the joint angles changes the end-effector position:

```
delta_position  is approximately  J(theta) @ delta_theta
```

For a 2-link arm, differentiating the forward kinematics gives

```
J = [ -l1 sin(t1) - l2 sin(t1+t2)    -l2 sin(t1+t2) ]
    [  l1 cos(t1) + l2 cos(t1+t2)     l2 cos(t1+t2) ]
```

To move the tip by a desired small error vector `e`, we solve `J delta_theta = e`. Because `J` may be nearly singular, we use the damped least squares (also called the Levenberg-Marquardt) form `delta_theta = J^T (J J^T + lambda^2 I)^(-1) e`, which stays well behaved near singular configurations.

```python
def jacobian_2link(t1, t2, l1, l2):
    s1, c1 = np.sin(t1), np.cos(t1)
    s12, c12 = np.sin(t1 + t2), np.cos(t1 + t2)
    return np.array([[-l1 * s1 - l2 * s12, -l2 * s12],
                     [ l1 * c1 + l2 * c12,  l2 * c12]])

def ik_iterative(target, theta_init, l1, l2, damping=0.05, tol=1e-6, max_iter=200):
    theta = np.array(theta_init, dtype=float)
    target = np.array(target, dtype=float)
    for i in range(max_iter):
        pos = np.array(forward_kinematics_2link(theta[0], theta[1], l1, l2))
        error = target - pos
        if np.linalg.norm(error) < tol:
            return (theta + np.pi) % (2 * np.pi) - np.pi, i      # wrap angles to [-pi, pi)
        J = jacobian_2link(theta[0], theta[1], l1, l2)
        delta = J.T @ np.linalg.solve(J @ J.T + damping**2 * np.eye(2), error)
        theta = theta + delta
    return (theta + np.pi) % (2 * np.pi) - np.pi, max_iter

theta, iters = ik_iterative((1.0, 1.2), theta_init=[0.1, 0.1], l1=l1, l2=l2)
print("converged in", iters, "iterations")
print("theta (deg):", np.rad2deg(theta).round(2))
print("tip:", np.round(forward_kinematics_2link(theta[0], theta[1], l1, l2), 5))
```

The iteration converges to one of the two analytical solutions, and which one depends on the starting guess. Try it from different starts.

```python
for init in ([0.1, 0.1], [1.2, -0.3], [1.0, -1.0], [-1.0, 1.0]):
    theta, iters = ik_iterative((1.0, 1.2), init, l1, l2)
    print(f"start {init}  ->  theta (deg) {np.rad2deg(theta).round(1)}  after {iters} iterations")
```

A starting point with positive `theta2` finds the elbow-down solution, and one with negative `theta2` finds the elbow-up one. For a real arm, a good starting guess is the current joint configuration. Then the iteration finds the solution nearest to where the arm already is, which avoids sudden jumps between elbow configurations.

What happens for an unreachable target? The iteration cannot converge, but it still does something useful: it pulls the arm as close as it can.

```python
theta, iters = ik_iterative((2.5, 0.0), [0.5, 0.5], l1, l2)
tip = forward_kinematics_2link(theta[0], theta[1], l1, l2)
print("iterations used:", iters, "(the maximum)")
print("tip ends at", np.round(tip, 3), "but the target was (2.5, 0)")
print("final error:", round(float(np.hypot(2.5 - tip[0], 0.0 - tip[1])), 3), "m")
```

The loop uses all 200 iterations and stalls with a large error. Here it has not even reached full stretch, because the steps become tiny near the singular, fully stretched shape. Whatever the details, the lesson is that an iterative IK routine never raises an error by itself, so the caller must check the final error, and report failure when it exceeds the tolerance.

## 4. Workspace Visualization

Sweeping every combination of joint angles through forward kinematics traces out the reachable region.

```python
import matplotlib.pyplot as plt

xs, ys = [], []
for t1 in np.linspace(0, 2 * np.pi, 100):
    for t2 in np.linspace(-np.pi, np.pi, 100):
        x, y = forward_kinematics_2link(t1, t2, l1=1.0, l2=0.8)
        xs.append(x)
        ys.append(y)

plt.scatter(xs, ys, s=1, color="gray")
plt.scatter([1.0, 1.0], [1.2, -1.2], color="black", marker="x")
plt.axis("equal")
plt.xlabel("x (m)")
plt.ylabel("y (m)")
plt.title("Reachable workspace of a 2-link arm (l1 = 1.0, l2 = 0.8)")
plt.show()

r = np.hypot(xs, ys)
print(f"smallest distance from base: {r.min():.3f}   largest: {r.max():.3f}")
```

The picture is a ring, which agrees with the condition `|l1 - l2| <= d <= l1 + l2`. The smallest distance should print as 0.2 and the largest as 1.8. This is a useful sanity check before attempting IK on a given target: a quick look tells you whether the request is sensible.

In practice real joints also have limits, for example an elbow that bends only through 150 degrees. Those limits remove part of the ring and may eliminate one of the two elbow solutions. A complete IK function should therefore filter its solutions by joint limits.

```python
def within_limits(sol, limits):
    return all(lo <= a <= hi for a, (lo, hi) in zip(sol, limits))

limits = [(np.deg2rad(-90), np.deg2rad(180)), (np.deg2rad(0), np.deg2rad(150))]   # elbow cannot bend backwards
candidates = inverse_kinematics_2link(1.0, 1.2, l1, l2)
usable = [s for s in candidates if within_limits(s, limits)]
print(f"{len(candidates)} analytic solutions, {len(usable)} within joint limits")
for t1, t2 in usable:
    print("  usable:", np.rad2deg([t1, t2]).round(2))
```

## 5. In-Class Exercise

For a given 2-link arm and target point, compute both IK solutions (elbow up and elbow down) by hand, then verify with `inverse_kinematics_2link`. Identify one unreachable target and explain why using the `d` calculation.

Use `l1 = 0.6`, `l2 = 0.4` and the target `(0.7, 0.3)`.

1. Compute `d^2 = 0.49 + 0.09 = 0.58`, so `d = 0.7616`. The range is from 0.2 to 1.0, so it is reachable.
2. `cos(theta2) = (0.58 - 0.36 - 0.16) / (2 * 0.6 * 0.4) = 0.06 / 0.48 = 0.125`, so `theta2 = +-82.82` degrees.
3. Finish the calculation of `theta1` for both signs of `theta2`.

```python
sols = inverse_kinematics_2link(0.7, 0.3, 0.6, 0.4)
for t1, t2 in sols:
    fx, fy = forward_kinematics_2link(t1, t2, 0.6, 0.4)
    print(np.rad2deg([t1, t2]).round(2), "->", round(fx, 4), round(fy, 4))

print(inverse_kinematics_2link(1.2, 0.0, 0.6, 0.4))   # beyond reach: d = 1.2 > 1.0
```

Questions:

1. For the unreachable point `(1.2, 0)` say which inequality fails, and by how much.
2. Is there any target for which this arm has exactly one solution? Which `d` values give it?
3. Run `ik_iterative` for `(0.7, 0.3)` from three starting guesses. Which analytical solutions do you get?

## 6. Common Mistakes

1. Using `arccos` without checking reachability first, or without clipping, which leads to `nan` values.
2. Returning only one of the two solutions without saying which one, or which convention is used.
3. Not verifying the answer with forward kinematics. This test takes one line and catches most errors.
4. Using `np.arctan` instead of `np.arctan2`, which loses the quadrant of the target.
5. Ignoring joint limits, and handing the robot angles that it physically cannot reach.
6. Taking a large iterative step near a singular configuration, so the arm swings wildly. Damping helps.

## 7. Summary

Inverse kinematics finds the joint settings that place the end-effector at a target, and unlike forward kinematics it can have two solutions, one, or none. For a 2-link planar arm the law of cosines gives both solutions directly, and the reachability test follows from the triangle inequality. For more complex arms a Jacobian-based iteration reaches a solution step by step, in much the same spirit as gradient descent. Always check an answer by running it forward again, and check the joint limits.

## 8. Practice Problems

1. For the arm with `l1 = 1`, `l2 = 0.8`, find the joint angles to draw a straight horizontal line from `(0.8, 0.5)` to `(1.4, 0.5)`, in ten steps. Pick the elbow-down solution each time, and plot the arm in each pose.
2. Use `ik_iterative` to follow the same line, but starting each step from the previous solution. Compare the angles with those from the analytical version.
3. Add a third link to the arm, and use the iterative method with a 3-column Jacobian to reach a target. How many solutions do you find from different starting guesses?
4. Show that the analytical IK breaks down when `l2` is zero. What would you do about it?

## 9. Suggested Reading

1. Lynch and Park, Modern Robotics, the chapter on inverse kinematics.
2. Buss, "Introduction to Inverse Kinematics with Jacobian Transpose, Pseudoinverse and Damped Least Squares Methods", a short and readable tutorial.
