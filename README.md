# Sahel Cloud Streets & Surface Energy Analysis

**Author:** Madison Emehel
**REU:** UW-Madison STORM, Dr. Bee Leung's Lab
**Status:** Active Research Project (Summer 2026)

## Overview
This repository contains demonstration code and methodological documentation for my research on cloud street formation over the Sahel. The project investigates how these organized cloud formations interact with the surface energy budget, with implications for regional climate and agriculture.

## Research Protection Notice
**This is a demonstration repository.** The actual analysis scripts for this project utilize unpublished model outputs. To protect the integrity of ongoing, unpublished research, only generic demonstration code is provided here, which illustrates the structure and logic of my workflow.

## Key Research Questions
- What atmospheric conditions drive cloud street formation over the Sahel?
- How does cloud street formation interact with the surface energy budget (net radiation, sensible/latent heat fluxes)?
- Does this relationship vary with pre-existing soil moisture levels?

## Repository Contents
- `SEB+BR_sample.ipynb`: A Jupyter notebook demonstrating my workflow.

## 🔬 Analysis Methodology

The core logic demonstrated in `SEB+BR_sample.ipynb` follows a 5-step workflow to analyze land-atmosphere interactions across 5 distinct soil moisture thresholds (0% to 100% SM):

* **Step 1: Data Ingestion & Lazy Loading**  
Model outputs are loaded from the target directory using custom data utilities. Files are handled using `xarray` to allow for efficient evaluation of multi-dimensional atmospheric grids.
* **Step 2: Core Variable Extraction**  
For each file, the following Surface Energy Budget variables are selected:
  * `SWDN` / `SWUP`: Downwelling and Upwelling shortwave radiation
  * `LWDN` / `LWUP`: Downwelling and Upwelling longwave radiation
  * `SFLUX_T`: Sensible heat flux
  * `SFLUX_R`: Latent heat flux
* **Step 3: Horizontal Domain Averaging**  
To evaluate the column energy balance, I slice the variables at the lowest vertical grid level (`z=1`) and average them across the horizontal domain using `ds.sel(z=1).mean(dim=("x", "y"))` to produce a single time series[cite: 1].
* **Step 4: Thermodynamic Conversions & Energy Conservation**  
All variables are converted to standard energy flux units ($W/m^2$):
  * **Net Shortwave ($SW_{net}$):** $SWDN - SWUP$
  * **Net Longwave ($LW_{net}$):** $LWDN - LWUP$
  * **Sensible Heat Flux ($SHF$):** $-SFLUX\_T \times 1004 \text{ J kg}^{-1}\text{ K}^{-1}$ (Specific heat of air, $C_p$)
  * **Latent Heat Flux ($LHF$):** $-SFLUX\_R \times 2.5 \times 10^6 \text{ J kg}^{-1}$ (Latent heat of vaporization, $L_v$)
  * **Ground Heat Storage ($G$):** Calculated as the residual of the surface energy balance equation to ensure strict energy conservation: $G = -(SW_{net} + LW_{net} + SHF + LHF)$
* **Step 5: Diurnal Time-Series Visualization**  
The resulting datasets are compiled using `pandas` and plotted with `matplotlib` to evaluate the diurnal partitioning of energy fluxes.

## Technical Skills Demonstrated
- **Languages:** Python
- **Libraries:** NumPy, Matplotlib, Pandas, Xarray
- **Domain Knowledge:** Surface energy budget theory, atmospheric physics.

## Acknowledgements
This work is supported by the UW-Madison STORM REU and advised by Dr. Bee Leung.
