# Calibrating an Epidemic Model with Emulators

An end-to-end calibration project on **real outbreak data**: the influenza epidemic at an English boys' boarding school in January–February 1978 (British Medical Journal, 1978; 763 boys, daily counts in bed and convalescent). A stochastic S-E-I-C-R simulator is calibrated with Gaussian-process emulators and history matching, and checked against brute-force ABC.

| File | What it is |
|---|---|
| `epidemic_calibration.ipynb` | The notebook (about 5 minutes). |
| `datasets/influenza_england_1978_school.csv` | The data (from the R package `outbreaks`). |
| `animations/epidemic_lab.html` | The stochastic simulator running in the browser (exact binomial sampler), the history-matching waves and posterior, and an interactive intervention explorer. |

## Highlights

- **Vectorized stochastic binomial-chain simulator:** fizzled outbreaks are handled explicitly, and summaries use a square-root (variance-stabilizing) transform.
- **GP emulators:** they model both the mean, with known replicate noise passed through `alpha`, and the stochastic variance. Validated on fresh runs, they are slightly overconfident (standardized std 1.13).
- **History matching in three waves:** the implausibility measure (three-sigma rule) cuts the parameter space to 18% of the prior box.
- **Emulator-based synthetic-likelihood posterior:** it agrees with 100,000-run ABC using **35× fewer simulator runs** (2,880).
- **Identifiability:** time in bed (about 2.1 days) and convalescence (about 1.5 days) are sharp. The transmission rate and latent period trade off along a ridge, so R₀ is weakly identified.
- **Model discrepancy:** the calibrated model fits both curves but infects all 763 boys, while 512 were reported ill. Homogeneous mixing is inadequate (Kennedy–O'Hagan).
- **Counterfactual interventions with uncertainty:** an early 80% contact cut would have kept the median peak at about 120 boys in bed.
