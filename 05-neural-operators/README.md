# Neural Operators

An end-to-end project on **neural operators**: learning the solution operator of the viscous Burgers equation, the map from an initial condition u(x, 0) to u(x, 0.5), with a Fourier neural operator (FNO) built from scratch in PyTorch.

| File | What it is |
|---|---|
| `neural_operator_burgers.ipynb` | The notebook (PyTorch, CPU only, about 45 minutes; most of the time goes to the Gaussian-process baseline and the learning curves). |
| `datasets/burgers_nu0005.npz` | The 1,200 cached simulations (1,024-point grid). |
| `animations/fno_explorer.html` | The pseudo-spectral solver and the trained FNO (exact weights) running in the browser: solver vs FNO race, any-resolution evaluation, and a look inside a Fourier layer. |

## Highlights

- **Verified simulator:** an integrating-factor RK4 pseudo-spectral solver with 2/3 dealiasing, checked against the exact Cole–Hopf solution, with spectral convergence down to 10⁻⁹.
- **Inputs:** Gaussian-random-field initial conditions, the standard neural-operator benchmark distribution.
- **Accuracy:** the FNO reaches **0.44%** test error (relative L2). The vector-based baselines are far behind: linear 68%, PCA + GP 20.5%, MLP 20.1%. With only 100 training simulations the FNO already reaches 5.6%.
- **Zero-shot super-resolution:** trained at 256 points, the FNO keeps the same error from 64 to 1,024 points. It is about 170× faster than the solver.
- **Out of distribution:** rougher or larger inputs degrade the error 3–6×.
- **Inverse problem through the differentiable FNO:** a naive search fools the surrogate (26% true misfit). A PCA prior keeps it honest (1%). The recovered initial condition is still non-unique, because dissipation destroys information.
