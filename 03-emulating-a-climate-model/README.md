# Emulating a Climate Model

An end-to-end emulation project on **real Earth-system-model output**: ClimateBench v1.0 (Watson-Parris et al., 2022), which contains emissions and annual temperature maps from NorESM2-LM (CMIP6) for seven scenarios. The emulator predicts warming maps from emissions in milliseconds, instead of months of supercomputer time.

| File | What it is |
|---|---|
| `climate_emulator.ipynb` | The notebook (about 10 minutes). It includes the code that downloads and reduces the original 900 MB of netCDF files. |
| `datasets/climatebench/climatebench_reduced.npz` | A 12 MB reduced copy of the data, used if present. |
| `animations/climate_emulator.html` | The project's GP emulator running in the browser: a net-zero scenario builder, a world warming map for any year, and a carbon-budget sweep. |

## Highlights

- **Physics from the data:** NorESM2's warming is nearly proportional to cumulative CO₂, with a TCRE of 0.45 K per 1,000 GtCO₂. Aerosols mask part of the warming (−0.7 K in the aerosol-only run).
- **Noise floor:** from internal variability, measured with ensemble members. No emulator can beat 0.32 K on the maps or 0.08 K on the global mean for 2080–2100.
- **Inputs and models:** aerosol emission maps are compressed into EOFs (PCA). The models compared are pattern scaling, ridge regression, random forest, MLP and a Gaussian process. Making the GP work for 3,456 outputs needed a shared kernel learned on the global mean, PCA outputs and a cell-level noise term (sklearn's gradient would need 219 GB).
- **Leave-one-scenario-out cross-validation:** the GP is best at interpolation but fails to extrapolate to SSP5-8.5. Linear models are hard to beat.
- **Held-out SSP2-4.5:** the best emulators come within 0.05–0.07 K of the noise floor on the maps.
- **Uses:** net-zero scenarios (each decade of delay adds about 0.04 K of peak warming in NorESM2) and carbon budgets. The budget section shows where the GP and pattern scaling disagree, and why the simpler model is the one to trust there.
