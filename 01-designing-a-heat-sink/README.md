# Designing a Heat Sink with a Surrogate Model

The task: find the lightest aluminium plate-fin heat sink that keeps a 100 W processor below 85 °C, using a fan with a realistic fan curve. 

| File | What it is |
|---|---|
| `heat_sink_surrogate.ipynb` | The notebook (about 5 minutes): the simulator, every output and every figure. |
| `datasets/` | The 60 training and 40 test simulator runs (CSV). |
| `animations/heat_sink_designer.html` | Interactive designer running the final surrogate in the browser: design studio, fan operating point and a live sweep over the fin count, and a replay of the optimizer race. |

## The simulator

A 3D finite-volume conduction solver (vectorized NumPy and SciPy sparse), with convection from the Teertstra–Yovanovich–Culham plate-fin correlation and a fan/duct operating point from the Muzychka–Yovanovich developing-flow friction model. It is checked against 1D fin theory (agreement within 1.4%) and with a grid convergence study.

## Highlights

- Adding physics features, including the cheap 1D fin theory, cuts the GP's cross-validation error from 10.4% to 0.36%. On the test set the final error is 0.23%, with honest 95% intervals.
- Multi-fidelity learning curves: with 10 expensive runs, the physics-informed models beat a plain GP trained on 60.
- Constrained Bayesian optimization finds a verified 126 g design in 40 runs, against 140 g for random search after 400 runs. Optimizing directly on the surrogate finds a verified 122 g design.
- The "1D theory + 30% margin" method proposes an 81 g design that actually runs 14 °C too hot. This shows how optimizers exploit model errors.
- Sobol' indices, a verified Pareto front of mass against temperature, and a deployable surrogate that refuses to extrapolate.
