# Lab Manual 12 — 1D Kalman Filter

**Duration:** 3 hours | **Prerequisite:** Week 12 lecture

## Objectives
Implement a 1D Kalman filter fusing a noisy position sensor with a motion model.

## Setup
Create `lab12.ipynb`; use instructor-provided simulated noisy measurements + ground truth motion.

## Procedure
1. **Task A — Implementation:** implement `KalmanFilter1D` as shown in lecture.
2. **Task B — Fusion:** run the filter over the provided motion/measurement sequence; plot raw
   measurements, filter estimate, and ground truth on one chart.
3. **Task C — Parameter sensitivity:** run the filter with 3 different `(process_var,
   measurement_var)` combinations; compare smoothness vs. responsiveness.
4. **Task D — Error analysis:** compute the mean absolute error between the filter's estimate
   and ground truth, and compare it to the mean absolute error of the raw measurements alone.

## Expected Output
A notebook with Tasks A–D, including the comparison plot and error numbers from Task D.

## Submission
Submit `lab12.ipynb` by the end of the lab session.
