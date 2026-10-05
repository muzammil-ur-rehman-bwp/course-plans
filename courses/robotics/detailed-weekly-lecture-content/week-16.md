# Week 16: Capstone, Course Review, Ethics and Safety

## Learning Objectives

By the end of this lecture, you should be able to:

1. Present a capstone robot behaviour clearly, with evidence that it works and honest limits.
2. Connect the techniques of the course into one picture, and trace a failure back to its source.
3. Calculate a safe speed from stopping distance, and implement a safety layer that overrides unsafe commands.
4. Record why a robot took an action, so that its decisions can be explained afterwards.
5. Discuss the privacy, social and professional issues of robotics in concrete terms.

## 1. Capstone Presentations

Each student or team presents for five to seven minutes, followed by questions, with a live or recorded simulation demonstration. The grading criteria are in `assignments/capstone-rubric.md`. Four things should be covered.

1. The behaviour you designed. What should the robot do, in what environment, and what counts as success? State it so that someone else could decide, from a video, whether it worked.
2. The techniques you applied, and why. Which parts of the course did you use: transforms, kinematics, ROS 2 nodes, control, vision, estimation, planning? A short justification of each choice is better than a long list.
3. The evaluation. What did you measure, over how many runs, and compared with what? One successful run shows that something is possible. Ten or twenty runs, with a summary, show how reliable it is.
4. The lessons learned. What failed, what you did about it, and what you would do next.

### 1.1 Advice for the demonstration

1. Prepare a recorded run as a backup. Live simulations fail at the worst moment.
2. Show a failure case on purpose, and explain it. It builds trust, and shows that you understand the system.
3. Put the key numbers on one slide: success rate, time, closest approach to obstacles.
4. Say what the robot knows and what it does not. For example, whether the map was given or discovered.
5. Practise with a timer.

### 1.2 Evaluating with more than one run

A robot is a stochastic system, since noise, timing and starting conditions vary. A single run is an anecdote. This small helper summarises a set of runs of the kind produced by the simulation of Week 15, and reports the success rate with an honest uncertainty. For a success rate measured from `n` trials, the Wilson interval is a reasonable way to state how much the number can be trusted.

```python
import math

def wilson_interval(successes, trials, z=1.96):
    """Approximate 95 percent confidence interval for a success rate."""
    if trials == 0:
        return (0.0, 1.0)
    p = successes / trials
    denom = 1 + z ** 2 / trials
    centre = (p + z ** 2 / (2 * trials)) / denom
    half = z * math.sqrt(p * (1 - p) / trials + z ** 2 / (4 * trials ** 2)) / denom
    return max(0.0, centre - half), min(1.0, centre + half)

def summarise_runs(runs):
    """runs: list of dicts with keys reached (bool), time_s, min_clearance_m."""
    n = len(runs)
    ok = [r for r in runs if r["reached"]]
    lo, hi = wilson_interval(len(ok), n)
    print(f"runs: {n}   successes: {len(ok)}   success rate {len(ok) / n:.2f}  (95% interval {lo:.2f} to {hi:.2f})")
    if ok:
        times = [r["time_s"] for r in ok]
        mean = sum(times) / len(times)
        sd = (sum((t - mean) ** 2 for t in times) / max(1, len(times) - 1)) ** 0.5
        print(f"time to goal: mean {mean:.1f} s, standard deviation {sd:.1f} s")
    print(f"closest approach to any obstacle: {min(r['min_clearance_m'] for r in runs):.2f} m")

# made-up numbers, in the same format that the Week 15 simulation produces
runs = [{"reached": True, "time_s": 26.0 + i % 3, "min_clearance_m": 0.35 + 0.02 * (i % 4)} for i in range(6)]
summarise_runs(runs)
print()
for k, n in [(6, 6), (18, 20), (60, 60)]:
    lo, hi = wilson_interval(k, n)
    print(f"{k:2d} successes in {n:2d} runs: the true success rate could be anywhere from {lo:.2f} to {hi:.2f}")
```

Look at the last lines. Six successes in six runs sounds perfect, but the data are consistent with a true success rate as low as about 0.61. Sixty out of sixty narrows it to above 0.94. If the robot will work near people, the number of trials you need is far larger than most course projects can afford, and you should say so in the presentation.

## 2. Course Review: The Big Picture

```
Math foundations: frames & transforms (Week 2)
        |
        v
Kinematics: forward & inverse (Weeks 3-4)
        |
        v
ROS 2 middleware (Weeks 5-6) + Dynamics/actuators (Week 7)
        |
        v
Sensing: encoders/IMU/odometry, range sensors (Weeks 8-9)
        |
        v
Control (PID, Week 10) + Vision (Week 11) + State estimation (Week 12)
        |
        v
Path planning: grid-based & sampling-based (Weeks 13-14)
        |
        v
Integration: full navigation pipeline (Week 15)
        |
        v
Capstone: an original autonomous robot behavior (Week 16)
```

The course is cumulative, not a sequence of separate topics. The tools of each phase return in the later ones.

1. Transforms from Week 2 turned a laser scan into world points in Week 9, and a camera detection into a bearing in Week 11.
2. The differential-drive model of Week 3 became the odometry of Week 8, the simulated robot of Week 6, and the vehicle in the simulation of Week 15.
3. The A* of the classical AI course became the grid planner of Week 13, and the potential field of Week 14 had the same local minimum problem as hill climbing.
4. The feedback idea of Week 1, the robot that stopped at the wall, became the PID controller of Week 10, and the damping of Week 12's filter.

### 2.1 Tracing a failure

A useful way to review is to take a symptom and trace it to a likely cause. Here is a short exercise, with the typical answers shown afterwards. For each symptom, name the week whose material you would look at first.

1. The robot reaches the goal in simulation but arrives 2 metres away on the real robot.
2. The robot oscillates left and right while following a straight corridor.
3. The planner reports that there is no path, though a human can see the way.
4. The arm moves to a position that is a mirror image of the target.
5. A subscriber never receives messages, although the publisher is running.
6. The detected object jumps between positions whenever the lights change.

```python
symptoms = {
    "arrives 2 m away on the real robot": "Week 8 (odometry drift, calibration) and Week 12 (fusing a correcting sensor)",
    "oscillates in a corridor": "Week 10 (proportional gain too high, or too little derivative, or delay in the loop)",
    "no path, but a human sees one": "Week 13 (inflation too large, or spurious obstacles from a wrong pose in the map of Week 9)",
    "arm reaches the mirror image of the target": "Week 4 (the other elbow solution of the inverse kinematics)",
    "subscriber receives nothing": "Weeks 5 and 6 (topic name, message type, or QoS mismatch; use ros2 topic info)",
    "detection jumps with the lights": "Week 11 (fixed HSV thresholds, hue wrap-around, white balance)",
}
for symptom, where in symptoms.items():
    print(f"- {symptom}\n    look at: {where}")
```

Notice that most problems are not in one component. They arise at the boundaries: units, frames, timing and assumptions. The habit of checking each interface, with a quick test, is worth more than any one algorithm.

## 3. Robotics Ethics and Safety

### 3.1 Human and robot safety

A physical robot can cause harm: it can crush, strike, trap, or simply fall on someone. Even though our work is in simulation, it is the right time to build the habits of a safe design. Three principles are the foundation.

1. Make the safe state the default. If a command stops arriving, or a sensor fails, the robot should stop. We saw the watchdog in Week 6.
2. Add a safety layer that is independent of, and simpler than, the clever software. It does not plan. It only vetoes commands that would be unsafe, and so it is easy to test and to trust.
3. Use margins. Speed limits, obstacle clearance and torque limits should be set with room to spare.

A key quantity is the stopping distance. At speed `v`, the robot travels a reaction distance `v * t_r` before the controller reacts to a sensor reading, and then a braking distance `v^2 / (2 a)` with deceleration `a`.

```python
def stopping_distance(v, decel=1.0, reaction_time=0.2):
    return v * reaction_time + v ** 2 / (2 * decel)

def max_safe_speed(distance_to_obstacle, decel=1.0, reaction_time=0.2, margin=0.3):
    """The largest speed from which the robot can stop with `margin` metres to spare."""
    d = distance_to_obstacle - margin
    if d <= 0:
        return 0.0
    # solve v^2/(2a) + v*t_r = d  for v
    a_coeff, b_coeff, c_coeff = 1 / (2 * decel), reaction_time, -d
    return (-b_coeff + (b_coeff ** 2 - 4 * a_coeff * c_coeff) ** 0.5) / (2 * a_coeff)

print("speed   stopping distance")
for v in [0.2, 0.5, 1.0, 1.5, 2.0]:
    print(f"{v:4.1f} m/s   {stopping_distance(v):5.2f} m")

print()
print("distance to obstacle   highest safe speed")
for d in [0.3, 0.5, 1.0, 2.0, 4.0]:
    print(f"{d:14.1f} m   {max_safe_speed(d):6.2f} m/s")

assert abs(stopping_distance(max_safe_speed(2.0)) + 0.3 - 2.0) < 1e-9
```

The stopping distance grows with the square of the speed. A robot that stops in 0.23 metres from 0.5 metres per second needs 2.4 metres from 2 metres per second, ten times more for a speed only four times higher. This is why speed limits are the most effective safety measure there is.

### 3.2 A safety layer

A safety filter sits between the planner or controller and the motors. It looks at the closest laser reading in the direction of travel, and reduces the speed to the highest safe value, and to zero if the robot is too close.

```python
class SafetyFilter:
    def __init__(self, decel=1.0, reaction_time=0.2, margin=0.3, max_speed=0.6):
        self.decel, self.reaction_time, self.margin, self.max_speed = decel, reaction_time, margin, max_speed
        self.interventions = 0

    def filter(self, v_cmd, omega_cmd, front_range):
        v_allowed = min(self.max_speed, max_safe_speed(front_range, self.decel, self.reaction_time, self.margin))
        if v_cmd > v_allowed:
            self.interventions += 1
            return v_allowed, omega_cmd, "limited"
        return v_cmd, omega_cmd, "ok"

safety = SafetyFilter()
for v_cmd, front in [(0.5, 5.0), (0.5, 1.0), (0.5, 0.6), (0.5, 0.3), (-0.2, 0.3), (1.5, 5.0)]:
    v, w, status = safety.filter(v_cmd, 0.0, front)
    print(f"command {v_cmd:+.1f} m/s with {front:3.1f} m free ahead -> {v:+.2f} m/s  ({status})")
print("interventions:", safety.interventions)
```

Study the output. A command of 0.5 metres per second goes through while the obstacle is far enough away, and it is still allowed at 0.6 metres, because the safe speed there is about 0.5. At 0.3 metres, which equals the margin, the speed is cut to zero. For distances in between, the speed is reduced smoothly. A command to reverse is left alone in this simple version, since the filter only watches the front, and a real one would also look behind. A command above the speed limit is clipped.

We test the filter in a simulated approach to a wall, with a controller that wants to go at full speed.

```python
def approach_wall(use_filter, wall_at=6.0, dt=0.05, decel=1.0):
    x, v = 0.0, 0.0
    safety = SafetyFilter(decel=decel)
    for _ in range(int(40 / dt)):
        front = wall_at - x
        v_cmd = 1.0                                     # a controller that does not look
        if use_filter:
            v_cmd, _, _ = safety.filter(v_cmd, 0.0, front)
        # the robot follows the commanded speed, with limited acceleration and braking
        step = decel * dt
        v += max(-step, min(step, v_cmd - v))
        x += v * dt
        if x >= wall_at:
            return "COLLISION at speed %.2f m/s" % v
    return f"stopped {wall_at - x:.2f} m from the wall"

print("without a safety layer:", approach_wall(False))
print("with a safety layer:   ", approach_wall(True))
```

Without the layer, the robot drives into the wall at its full speed. With it, the robot stops short. The layer is only a few lines, and that is the point: the safety function must be so simple that it can be fully tested and understood, which the planner and estimator cannot be.

### 3.3 Autonomous decisions and explanation

As a robot acts with less human supervision, it matters more to be able to say why it did what it did. When something goes wrong, someone has to find out whether the sensor misled it, whether the estimate was wrong, whether the plan was bad or the controller failed. A decision log makes this possible. It records, at every decision, the inputs that were used, the action and the reason.

```python
class DecisionLog:
    def __init__(self):
        self.entries = []

    def record(self, t, inputs, action, reason):
        self.entries.append({"t": t, "inputs": inputs, "action": action, "reason": reason})

    def explain(self, t):
        e = min(self.entries, key=lambda e: abs(e["t"] - t))
        return f"At t = {e['t']:.1f} s the robot chose '{e['action']}' because {e['reason']} (inputs: {e['inputs']})"

log = DecisionLog()
safety = SafetyFilter()
for k, front in enumerate([4.0, 2.0, 1.0, 0.6, 0.35]):
    v, w, status = safety.filter(0.5, 0.0, front)
    reason = "the way ahead was clear" if status == "ok" else f"only {front} m was free ahead and the highest safe speed there is {v:.2f} m/s"
    log.record(t=k * 1.0, inputs={"front_range": front, "v_cmd": 0.5}, action=f"v = {v:.2f}", reason=reason)

print(log.explain(3.0))
print(log.explain(4.0))
```

A log is not an explanation of a learned model's inner workings, but for a pipeline built from the parts of this course, which is a chain of understandable steps, it is a great deal of help. It also supports accountability. If a robot harms someone, the people involved, and those who investigate, will ask who decided what. The engineers, the operators and the organisation that deploys the robot all carry some of the responsibility, and a record makes it possible to find out.

### 3.4 Data privacy

A robot's sensors are an observation system. A camera sees the faces, the screens and the papers in the rooms through which it passes, and a scanner builds an exact map of a home or an office. Questions that every project should ask:

1. What does the robot record, and does it need to? If the task is navigation, then the images need not be stored.
2. Who can see the data, where is it kept, and for how long?
3. Have the people in the space been told, and have they agreed?
4. Can the data be reduced at the source, for example by blurring faces or keeping only the occupancy grid?

The last is easy to show. Here a region of an image is blurred before storage.

```python
import numpy as np
import cv2

image = np.zeros((120, 160, 3), np.uint8)
cv2.circle(image, (80, 50), 25, (180, 200, 230), -1)           # a "face"
cv2.circle(image, (70, 45), 4, (20, 20, 20), -1)               # eyes
cv2.circle(image, (90, 45), 4, (20, 20, 20), -1)
cv2.rectangle(image, (70, 62), (90, 66), (20, 20, 120), -1)    # mouth

def anonymise(img, box):
    x, y, w, h = box
    out = img.copy()
    roi = out[y:y + h, x:x + w]
    out[y:y + h, x:x + w] = cv2.GaussianBlur(roi, (0, 0), sigmaX=12)
    return out

private = anonymise(image, (50, 25, 60, 55))
change = np.abs(private.astype(int) - image.astype(int)).sum(axis=2)
print("pixels changed inside the box: ", int((change[25:80, 50:110] > 0).sum()))
print("pixels changed outside the box:", int((change.sum() - change[25:80, 50:110].sum())))
print("eye detail before:", image[45, 70], " after:", private[45, 70])
```

The region is changed, the rest of the picture is untouched, and the detail of the eyes has gone. Blurring a region that has been found by a face detector is a typical privacy measure. A better one is not to keep the picture at all.

### 3.5 Societal and professional impact

Automation changes work. Robots that move goods in a warehouse, clean floors, drive vehicles or assist in surgery change what people do, and some jobs disappear while others appear. Engineers do not decide this alone, but they are not neutral either. We should know the context in which our systems will be used, who gains and who loses, and take part in the discussion.

Some professional habits that follow from this.

1. Be honest about what a system can do. A demonstration under ideal conditions is not a guarantee.
2. Test beyond the happy path, including failures, edge cases and misuse.
3. Report incidents and near misses, and learn from them.
4. Think of the people who will work with the robot, and those who will be near it by chance, and not only the person who buys it.
5. Know the standards and regulations of the field, such as those for industrial and service robot safety, and be prepared to follow them.

## 4. In-Class Exercise and Closing Discussion

Hold an open discussion on where each capstone project could go next: real hardware deployment, more sophisticated planning, or learned perception models.

Prepare for it with a short written answer to each of these, for your own project.

1. What is the most likely way that your robot could hurt someone or something, in the real world, and what safety measure addresses it?
2. If you deployed it on hardware next month, which assumption of the simulation would break first? Use the sim-to-real gap of Week 1 as a guide.
3. What data does it collect, and what is the least it needs?
4. What single experiment would tell you most about whether the design is good?

As a final practical task, check your own capstone against these rules.

```python
checklist = {
    "stops safely if commands or sensor data stop arriving": None,
    "has a speed limit tied to stopping distance": None,
    "has a safety layer independent of the planner": None,
    "was evaluated over many runs, with failures reported": None,
    "records why it acted (a decision log)": None,
    "stores no more data than the task needs": None,
}
# fill in True or False for your project, then run this cell
checklist.update({k: True for k in list(checklist)[:2]})
done = [k for k, v in checklist.items() if v]
todo = [k for k, v in checklist.items() if not v]
print(f"{len(done)} of {len(checklist)} items done")
for item in todo:
    print("  still to do:", item)
```

## 5. Summary

The course moved from coordinate frames to a complete navigating robot, and each stage used the tools of the one before. A good project is judged by evidence, from many runs, with honest limits. Ethics and safety belong to the engineering, not the end. Safe defaults, simple independent safety layers, stopping distance, explanation, privacy by design and a realistic view of the social effects are things that you can build into your systems from the start. Whatever you build next, simulation first, a clean architecture of nodes, and a habit of testing every interface will serve you well.

## 6. Final Practice Tasks

1. Add the `SafetyFilter` to the pipeline of Week 15, and measure whether it changes the success rate, the time and the closest approach.
2. Write a one page safety note for your capstone, with the hazards, the measures and the tests that show the measures work.
3. Run your capstone twenty times with different seeds, and report the success rate with its interval.
4. Replace the stored camera images in your project with a version that keeps only what the task needs.

## 7. Suggested Reading

1. ISO 10218 and ISO 13482, the safety standards for industrial and personal care robots, as an introduction to how the field regulates itself.
2. Lin, Abney and Bekey (editors), Robot Ethics: The Ethical and Social Implications of Robotics.
3. The course's academic integrity policy and the capstone rubric.
