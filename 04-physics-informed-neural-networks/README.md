# Physics-Informed Neural Networks

An end-to-end project with **physics-informed neural networks (PINNs)** on **real data**: hourly soil temperatures at 5, 10, 20, 50 and 100 cm from the NOAA U.S. Climate Reference Network station Nunn 7 NNE, Colorado, over three rain-free weeks (8–28 June 2023). The PINN learns the soil's thermal diffusivity and a continuous temperature field, and is compared with classical methods.

| File | What it is |
|---|---|
| `soil_pinn.ipynb` | The notebook (PyTorch, CPU only, about 15–20 minutes). |
| `datasets/uscrn/` | The station's 2023 hourly file and its column headers. |
| `animations/soil_heat_explorer.html` | The notebook's finite-difference solver running in the browser with the real boundary sensors (fit α by hand), the daily-wave explorer, and the PINN's temperature field. |

## Highlights

- **Ångström's daily-wave method, applied layer by layer:** the soil turns out to be layered. Its diffusivity rises about 9× between the top 10 cm and 20–50 cm (dry topsoil).
- **Four methods compete on a held-out sensor at 20 cm:**
  - finite-difference solver plus fitted α: 0.20 °C
  - PINN: 0.46 °C
  - data-only network: 1.45 °C
- **Training PINNs:** a naive PINN fails because the physics residual is about 100× too large in normalized time. Nondimensionalizing the residual fixes it. The project also covers Fourier features against spectral bias, a sweep of the physics weight λ, and an ensemble of seeds.
- **The honest lesson:** physics makes the network three times better with sparse data, but a good classical solver still wins on a simple 1D linear problem. A sidebar explains when PINNs shine.
- **Use:** the PINN gives the ground heat flux, which no sensor measures, from its exact gradient: up to about 45 W/m² at midday.
