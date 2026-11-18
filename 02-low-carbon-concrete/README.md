# Designing Low-Carbon Concrete

An end-to-end surrogate project on **real laboratory data**: the Concrete Compressive Strength dataset (I-Cheng Yeh, 1998; UCI Machine Learning Repository, CC BY 4.0). It contains 1,030 compression tests on 428 mixes. The surrogate stands in for 28-day lab experiments and is used to design concrete with less embodied CO₂.

| File | What it is |
|---|---|
| `low_carbon_concrete.ipynb` | The notebook (about 7 minutes): every output and every figure. |
| `datasets/concrete/concrete_data.csv` | A copy of the UCI data, so the notebook also runs offline. |
| `animations/concrete_mix_designer.html` | Interactive mix designer that runs the final model's 800 boosted trees in the browser, an Abrams' law explorer, and a replay of the lab campaign. |

## Highlights

- **Group leakage:** random cross-validation folds report 4.5 MPa, while folds grouped by mix give 5.9 MPa, because the same concrete is tested at several ages.
- **Rediscovering Abrams' law (1918) from data:** the strength falls exponentially with the water/binder ratio. A physics baseline (Abrams' law plus ACI 209 strength gain) is compared with linear, random forest, gradient boosting, MLP and GP models.
- **Monotonic constraints:** they cost 0.2 MPa of cross-validation accuracy but keep the surrogate physically sane, which matters when an optimizer drives it.
- **Split-conformal lower bounds:** the 95% bounds cover 95.4% of unseen test mixes. The final test RMSE is 5.2 MPa (R² = 0.90).
- **The optimizer's curse, found and fixed:** global screening of about a million virtual mixes promises 15–30% CO₂ savings. Nearest-neighbour checks and a GP second opinion expose them as extrapolation. A trust-region search, with both models agreeing, gives credible designs with **8–13% less CO₂** than the best real mixes certified by the same rule.
- **Retrospective Bayesian-optimization lab campaign on real tests:** a median of 229 kg CO₂/m³ for C40 after 40 tests, against 274 kg for random testing.
