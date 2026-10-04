# Week 11 Summary — Algorithmic Fairness

**Key takeaways:**
- Demographic parity, equalized odds, and calibration formalize three independently reasonable,
  but generally incompatible, notions of fairness.
- The PPV identity $\mathrm{PPV}=p\cdot\mathrm{TPR}/(p\cdot\mathrm{TPR}+(1-p)\cdot\mathrm{FPR})$ is
  strictly monotonic in the base rate $p$ for any non-degenerate classifier, so equal PPV under
  shared TPR/FPR forces equal base rates.
- The impossibility result: whenever group base rates differ, equalized odds and
  calibration/predictive parity cannot both hold (barring a degenerate classifier) — a genuine
  mathematical incompatibility, making fairness-criterion selection a value-laden choice.

**You should now be able to:** state the three formal fairness criteria; derive the
calibration/equalized-odds impossibility result via PPV monotonicity; compute fairness metrics and
empirically confirm the impossibility result in both directions.

**Next week:** Robust statistics — median-of-means and trimmed-mean estimation under
contamination, and the connection to adversarial robustness.
