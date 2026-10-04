# Week 12 Summary — State Estimation: Sensor Fusion & Kalman Filter

**Key takeaways:**
- Sensor fusion combines multiple imperfect sensors into one better estimate than any single
  sensor alone.
- The Kalman filter alternates predict (project forward via motion model, increasing
  uncertainty) and update (incorporate a measurement, decreasing uncertainty) steps.
- The Kalman gain balances trust between the motion model and new measurements based on their
  relative noise levels (`process_var` vs. `measurement_var`).

**You should now be able to:** implement and tune a 1D Kalman filter; explain the predict/update
cycle and the role of the Kalman gain.

**Next week:** path planning — using a planning algorithm (A*) on an occupancy grid built from
sensor data.
