# Week 12 — Lecture Content: State Estimation — Sensor Fusion & Kalman Filter

## 1. Why Fuse Sensors?
No single sensor is perfect: odometry drifts over time (Week 8), range/vision sensors are noisy
and only observe part of the state. **Sensor fusion** combines multiple noisy/partial sources
into a single, better state estimate.

## 2. The Kalman Filter: Predict/Update Cycle (1D)
For a 1D state (e.g., position along a line), the Kalman filter alternates:
- **Predict**: use the motion model to project the current estimate forward (increasing
  uncertainty, since the model is imperfect).
- **Update**: incorporate a new noisy measurement, weighted by relative confidence (the "Kalman
  gain"), to produce a refined estimate (decreasing uncertainty).

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
```
When measurement noise (`r`) is high relative to process noise (`q`), the filter trusts the
motion model more and the Kalman gain shrinks; when `r` is low, it trusts new measurements more.

## 3. Applying It
```python
kf = KalmanFilter1D(initial_estimate=0.0, initial_uncertainty=1.0,
                     process_var=0.01, measurement_var=0.5)
for motion, measurement in zip(motions, noisy_measurements):
    kf.predict(motion)
    kf.update(measurement)
    # kf.x is now the fused position estimate
```

## 4. Evaluating the Filter
Plot the raw noisy measurements, the filter's estimate, and (if available) ground truth on the
same axes — a well-tuned filter's estimate should visibly track ground truth more closely than
the raw noisy measurements, with less jitter.

## 5. In-Class Exercise
Starting from the provided `KalmanFilter1D`, adjust `process_var` and `measurement_var` and
observe the effect on how smooth vs. responsive the filtered estimate is.
