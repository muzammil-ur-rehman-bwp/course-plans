# Week 12: State Estimation, Sensor Fusion and the Kalman Filter

## Learning Objectives

By the end of this lecture, you should be able to:

1. Explain why a single sensor is rarely enough, and what sensor fusion does about it.
2. Combine two noisy measurements of the same quantity by weighting them by their reliability.
3. Describe the predict and update cycle of a Kalman filter, and the role of the Kalman gain.
4. Implement and run a one-dimensional Kalman filter, and compare it with raw measurements and with dead reckoning.
5. Tune the process and measurement variances, and recognise a filter that is too sluggish or too jittery.

## 1. Why Fuse Sensors?

Every sensor has a weakness. We saw in Week 8 that odometry is smooth and accurate over short distances but drifts without limit. A position sensor that sees external landmarks, such as a laser scanner matched to a map, or a satellite positioning receiver, has no drift, but each reading is noisy and sometimes arrives only a few times per second. A camera sees a lot, but only a part of the state, and only when the light is right.

Sensor fusion combines several such sources into a single estimate that is better than any of them alone. The general idea is simple. Use a model of how the robot moves to predict where it should be now, which is smooth but gradually wrong. Use measurements to pull that prediction back toward reality, which are unbiased but noisy. Give each as much weight as its reliability deserves.

The word state refers to the quantities we want to know: position, heading, speed. State estimation is the problem of inferring the state from imperfect measurements. The Kalman filter is the classic solution when the models are linear and the noise is roughly Gaussian, and it remains the basis of most estimators in robots, aircraft and phones.

## 2. Combining Two Measurements

Start with the simplest case, no motion at all. Two sensors measure the same distance. Sensor A says 10.0 m with a variance of 0.04 (a standard deviation of 0.2 m), and sensor B says 10.5 m with a variance of 0.16 (standard deviation 0.4 m). What is the best estimate?

An ordinary average gives 10.25, which treats the two as equally good. A better answer weights each measurement by the inverse of its variance, the precision, so that the more reliable sensor counts for more.

```
x_fused = (z_A / var_A + z_B / var_B) / (1 / var_A + 1 / var_B)
var_fused = 1 / (1 / var_A + 1 / var_B)
```

```python
import numpy as np

def fuse(z1, var1, z2, var2):
    w1, w2 = 1 / var1, 1 / var2
    x = (z1 * w1 + z2 * w2) / (w1 + w2)
    return x, 1 / (w1 + w2)

x, var = fuse(10.0, 0.04, 10.5, 0.16)
print(f"fused estimate {x:.3f} m, variance {var:.4f} (std {np.sqrt(var):.3f} m)")
print(f"plain average {np.mean([10.0, 10.5]):.3f} m")
```

The fused estimate, 10.1 m, lies four times closer to the better sensor, since its variance is four times smaller. The fused variance, 0.032, is smaller than both of the inputs. That is the whole point: combining information reduces uncertainty, and never increases it.

We can check this by simulation. Draw many pairs of readings, and compare the errors of the two methods.

```python
rng = np.random.default_rng(0)
truth = 10.0
zA = truth + rng.normal(0, 0.2, 100000)
zB = truth + rng.normal(0, 0.4, 100000)
fused = (zA / 0.04 + zB / 0.16) / (1 / 0.04 + 1 / 0.16)
avg = (zA + zB) / 2

for name, est in [("sensor A", zA), ("sensor B", zB), ("plain average", avg), ("variance weighted", fused)]:
    print(f"{name:18s} standard deviation of the error: {est.std():.4f} m")
```

The weighted combination has the smallest error, about 0.18 m, better than the better sensor alone. The Kalman filter is this same weighting idea, applied repeatedly through time, with the prior estimate playing the part of one of the sensors.

## 3. The Kalman Filter: Predict and Update

The filter keeps two numbers for each state variable: the estimate `x`, and its variance `p`, which measures how uncertain it is. It alternates two steps.

1. Predict. Use the motion model to move the estimate forward. If the robot was told to move 0.1 m, the estimate moves by 0.1 m. The model is not perfect, since wheels slip, so the uncertainty grows. We add the process variance `q` to `p`.
2. Update. Take a new measurement `z` with measurement variance `r`. Compute how much to trust it, the Kalman gain `K`, and move the estimate toward the measurement by that fraction of the difference. The uncertainty falls.

```
Predict:   x = x + motion          p = p + q
Update:    K = p / (p + r)
           x = x + K * (z - x)     p = (1 - K) * p
```

Look at the gain. If our estimate is very uncertain (`p` large) and the measurement is precise (`r` small), then `K` is near 1, and we nearly adopt the measurement. If the estimate is confident and the measurement is noisy, `K` is near 0, and we nearly ignore the measurement. That is what the weighting in section 2 did. The update step is exactly the `fuse` function with the estimate and the measurement as the two inputs.

```python
class KalmanFilter1D:
    def __init__(self, initial_estimate, initial_uncertainty, process_var, measurement_var):
        self.x = initial_estimate
        self.p = initial_uncertainty
        self.q = process_var          # process (model) noise
        self.r = measurement_var      # measurement noise

    def predict(self, motion=0.0):
        self.x += motion
        self.p += self.q

    def update(self, measurement):
        k = self.p / (self.p + self.r)    # Kalman gain
        self.x += k * (measurement - self.x)
        self.p *= (1 - k)
        return k
```

The only change from the minimal version is that `update` returns the gain, so that we can look at it.

Check that the update really is the weighted fusion of section 2.

```python
kf = KalmanFilter1D(initial_estimate=10.0, initial_uncertainty=0.04, process_var=0.0, measurement_var=0.16)
kf.update(10.5)
print(f"Kalman update: x = {kf.x:.3f}, p = {kf.p:.4f}   (fusion gave 10.100 and 0.0320)")
```

When measurement noise `r` is large compared with the process noise `q`, the filter trusts its motion model more, and the gain becomes small. When `r` is small, the filter trusts new measurements more.

## 4. Applying the Filter

We need a scenario in which fusion helps. A robot drives along a line at 0.1 m per step for 300 steps. Its odometry has a calibration problem, it reports 5 percent more distance than the robot really moved, plus some random noise. A second sensor measures the position directly but with a noise of 0.5 m per reading, which is large.

```python
def simulate(n=300, step=0.1, odo_scale=1.05, odo_noise=0.02, meas_noise=0.5, seed=0):
    rng = np.random.default_rng(seed)
    truth = np.cumsum(np.full(n, step))
    motions = step * odo_scale + rng.normal(0, odo_noise, n)    # what the odometry reports at each step
    measurements = truth + rng.normal(0, meas_noise, n)         # the noisy position sensor
    return truth, motions, measurements

def run_filter(q, r, motions, measurements, x0=0.0, p0=1.0):
    kf = KalmanFilter1D(x0, p0, q, r)
    estimates, gains = [], []
    for motion, z in zip(motions, measurements):
        kf.predict(motion)
        gains.append(kf.update(z))
        estimates.append(kf.x)
    return np.array(estimates), np.array(gains)

def rmse(a, b):
    return float(np.sqrt(np.mean((np.asarray(a) - np.asarray(b)) ** 2)))

truth, motions, measurements = simulate()
dead_reckoning = np.cumsum(motions)
estimates, gains = run_filter(q=0.002, r=0.25, motions=motions, measurements=measurements)

print(f"raw measurements:  RMS error {rmse(measurements, truth):.3f} m")
print(f"dead reckoning:    RMS error {rmse(dead_reckoning, truth):.3f} m   (final error {dead_reckoning[-1] - truth[-1]:.2f} m)")
print(f"Kalman filter:     RMS error {rmse(estimates, truth):.3f} m")
print(f"gain: first step {gains[0]:.2f}, after 10 steps {gains[9]:.2f}, at the end {gains[-1]:.3f}")
```

The measurement variance is `r = 0.25`, the square of the sensor's standard deviation of 0.5. The result: the raw measurements are off by about 0.5 m, dead reckoning gets steadily worse and ends more than a metre away, while the filter stays within about 0.12 m. Neither input is good, but the combination is. Odometry supplies the smoothness, and the position sensor stops the drift.

The gain starts large, since at first the filter knows little, and settles to a small value as confidence builds. It does not go to zero, because the process variance `q` keeps adding some uncertainty at each step.

A single run could be luck. Repeat over many random seeds.

```python
results = {"raw": [], "dead": [], "kf": []}
for seed in range(100):
    t, m, z = simulate(seed=seed)
    est, _ = run_filter(0.002, 0.25, m, z)
    results["raw"].append(rmse(z, t))
    results["dead"].append(rmse(np.cumsum(m), t))
    results["kf"].append(rmse(est, t))
print({k: round(float(np.mean(v)), 3) for k, v in results.items()})
```

On average over 100 runs the errors are about 0.50 m for raw measurements, 0.91 m for dead reckoning and 0.13 m for the filter.

### 4.1 Recovering from a bad start

A good filter also copes with a wrong initial guess, if it knows it is unsure. We start the filter at 5 m, when the robot is really at 0, but tell it that it is very uncertain, with an initial variance of 100.

```python
est_bad, g_bad = run_filter(0.002, 0.25, motions, measurements, x0=5.0, p0=100.0)
print("step   estimate   truth   gain")
for k in [0, 1, 2, 5, 10, 30]:
    print(f"{k:4d}   {est_bad[k]:8.3f}  {truth[k]:6.2f}  {g_bad[k]:5.2f}")
```

At the first step the gain is almost 1, so the filter jumps to the measurement and forgets its absurd initial guess. If it had been told that the initial guess was reliable, with a variance of 0.01, it would take much longer to correct itself. Honest initial uncertainty matters.

## 5. Evaluating and Tuning the Filter

To judge a filter, we plot the raw measurements, the estimate and, in simulation, the ground truth together.

```python
import matplotlib.pyplot as plt

steps = np.arange(len(truth))
plt.scatter(steps, measurements, s=6, color="lightgray", label="noisy measurements")
plt.plot(steps, dead_reckoning, color="gray", linestyle=":", label="dead reckoning")
plt.plot(steps, truth, color="black", linewidth=1, label="truth")
plt.plot(steps, estimates, color="black", linestyle="--", label="Kalman estimate")
plt.xlabel("step")
plt.ylabel("position (m)")
plt.legend()
plt.title("Fusing drifting odometry with a noisy position sensor")
plt.show()
```

A well tuned filter follows the truth more closely than the measurements do, with less jitter, and without the growing drift of dead reckoning. The two tuning parameters mean the following.

1. The process variance `q` says how much we distrust the motion model. A small `q` means that we believe the odometry, and the filter becomes smooth, but slow to correct a drift. A large `q` means that we do not believe the odometry, so the filter follows the measurements, noise included.
2. The measurement variance `r` says how much we distrust the sensor. A large `r` gives a smooth, slow estimate, and a small `r` gives a quick, noisy one.

Only the ratio of `q` to `r` really matters for the gain in the long run. The experiment below changes both and records the error, the lag behind the truth, and the jitter, which we define as the standard deviation of the step-to-step changes of the estimate, compared with the true change of 0.1.

```python
print(f"{'q':>8} {'r':>6}   RMS error   lag(m)   jitter   final gain")
for q, r in [(0.0001, 0.25), (0.0004, 0.25), (0.002, 0.25), (0.01, 0.25), (0.05, 0.25), (0.0004, 0.05), (0.0004, 2.5)]:
    est, g = run_filter(q, r, motions, measurements)
    lag = float(np.mean(est[-50:] - truth[-50:]))
    jitter = float(np.std(np.diff(est) - 0.1))
    print(f"{q:8.4f} {r:6.2f}   {rmse(est, truth):9.3f}  {lag:7.3f}  {jitter:7.3f}  {g[-1]:10.3f}")
```

Read the first rows. With a tiny `q` of 0.0001, the filter trusts the odometry so much that it corrects slowly, and so it runs 0.16 m off the truth at the end. With `q` large, 0.05, the filter is quick, but its estimate jitters, and the error rises again. The best value here is around `q = 0.002`, where the lag has almost vanished, and the jitter is still moderate. The last row has `r = 2.5`, which says the sensor is terrible, so the filter ignores it, and the odometry drift comes back as a lag of 0.26 m.

A word on how to choose `q` and `r` in practice. The measurement variance can usually be measured, by logging a sensor at rest and computing the variance of its readings. The process variance is harder, and is often found by trial and error, guided by how well the motion model fits. Tuning `q` is part of the craft of using Kalman filters.

## 6. Where This Goes Next

This one dimensional filter keeps one number and one variance. A real robot estimates a vector of states, such as position, heading and velocity, and then `x` is a vector, `p` becomes a covariance matrix, and the scalar formulas become matrix equations of the same shape. If the motion or the measurement is not linear, for instance a heading that changes the direction of travel, the extended Kalman filter linearises the models at each step. We do not derive these in this course. The structure of predict, then update, with a gain that weights the model against the measurement, is the same.

In the navigation pipeline of Week 15 you will use the filter to combine odometry with whatever correction signal the simulation provides.

## 7. In-Class Exercise

Starting from the provided `KalmanFilter1D`, adjust `process_var` and `measurement_var`, and observe the effect on how smooth versus responsive the filtered estimate is.

A good approach is to add a test in which the truth changes suddenly, so that the responsiveness is visible. Here the robot's true position jumps by 2 metres at step 100, as if it had been picked up and moved (the kidnapped robot problem), and the odometry does not know about it.

```python
def jump_scenario(n=200, step=0.1, jump_at=100, jump=2.0, meas_noise=0.5, seed=1):
    rng = np.random.default_rng(seed)
    truth = np.cumsum(np.full(n, step))
    truth[jump_at:] += jump
    motions = np.full(n, step) + rng.normal(0, 0.02, n)     # odometry sees no jump
    measurements = truth + rng.normal(0, meas_noise, n)
    return truth, motions, measurements

t, m, z = jump_scenario()
print(f"{'q':>8} {'r':>6}   steps to recover to within 0.5 m after the jump")
for q, r in [(0.0004, 0.25), (0.002, 0.25), (0.01, 0.25), (0.05, 0.25), (0.002, 2.5)]:
    est, _ = run_filter(q, r, m, z)
    err = np.abs(est[100:] - t[100:])
    recovered = next((i for i in range(len(err)) if np.all(err[i:i + 10] < 0.5)), None)
    print(f"{q:8.4f} {r:6.2f}   {recovered if recovered is not None else 'did not recover'}")
```

The trade is clear. A filter with a small `q` believes its odometry and takes a long time to accept that the robot has been moved. One with a large `q` recovers within a few steps, at the cost of more jitter in normal operation. Write two sentences on which you would choose for a robot that is unlikely to be picked up and moved, and which for one that often slips on a wet floor.

Questions:

1. What happens to the gain if `q = 0`? Run the filter for a thousand steps and look at the gain at the end. Is that behaviour desirable?
2. If the sensor's real noise is 0.5 m but you tell the filter `r = 0.01`, what do you expect? Try it.
3. Why is it harmless to run the update step more often than the predict step?

## 8. Common Mistakes

1. Telling the filter the standard deviation when it wants the variance, or the reverse.
2. Starting with a very small initial uncertainty and a wrong initial estimate.
3. Setting `q = 0` so that the filter eventually stops listening to the measurements.
4. Tuning on one run and one seed.
5. Fitting the filter's noise values to make a plot look pleasing, without any physical basis.
6. Forgetting to convert both the prediction and the measurement into the same frame and units before fusing them.
7. Treating the output variance as ground truth. It is only correct if the models and noise values were correct.

## 9. Summary

No sensor is perfect, and fusing a smooth but drifting source with a noisy but unbiased one yields an estimate better than either. The Kalman filter does this by predicting with a motion model, which increases uncertainty, and updating with a measurement, which reduces it, weighting each by its variance through the Kalman gain. Tuning means choosing how much to trust the model against the sensor, and the right balance gives an estimate that is accurate, without being laggy or jittery. Next week we use the maps and poses that our sensing and estimation provide, for planning paths.

## 10. Practice Problems

1. Extend `KalmanFilter1D` to a two-state filter with position and velocity, using NumPy matrices. Fuse a noisy position sensor only, and check that the filter estimates the velocity too.
2. Show that the Kalman update with `p` and `r` is identical to the `fuse` function with variances `p` and `r`.
3. Simulate a position sensor that occasionally gives a wild outlier, and add a simple gate that rejects a measurement farther than three standard deviations from the prediction.
4. Plot the variance `p` against the step number for a few values of `q` and `r`, and describe how it reaches a steady value.

## 11. Suggested Reading

1. Thrun, Burgard and Fox, Probabilistic Robotics, the chapter on the Kalman filter.
2. Labbe, Kalman and Bayesian Filters in Python, a free book of notebooks.
