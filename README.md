# VHR Satellite Imagery for Agricultural Weed Mapping: *Sorghum halepense* Detection in Maize Crops

[![Python Version](https://img.shields.io/badge/python-3.10-blue.svg)](https://www.python.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Conda Environment](https://img.shields.io/badge/conda-environment-green.svg)](environment.yml)

## 📌 Project Overview
This repository contains the scientific workflow and automated pipeline developed to process, analyze, and map infestations of Johnsongrass (*Sorghum halepense*) within maize fields at La Poveda (Madrid, Spain). 

Leveraging multi-source **Very High Resolution (VHR)** satellite imagery—specifically **WorldView-2 (0.5m/2.0m)** and **PlanetScope (3.0m)**—the project evaluates spectral separability, conducts radiometric and atmospheric corrections (DOS1), and applies dimensional reduction and classification algorithms (PCA, LDA, Random Forest) to assess the impact of spatial resolution on agricultural weed discrimination.

---

## 🛠️ Methodological Workflow
The project implements a structured three-step workflow:
1. **Input Data Integration:** UAV orthomosaics (ground truth) and co-registered VHR satellite datasets (WorldView-2 & PlanetScope).
2. **Preprocessing Pipeline:** Geometric adjustment (centroid-based parcel alignment), Top-of-Atmosphere (TOA) conversion, and Bottom-of-Atmosphere (BOA) DOS1 atmospheric path radiance correction.
3. **Statistical & Predictive Analysis:** Spectral signature extraction, ANOVA & Tukey’s HSD post-hoc testing, and machine learning-based classification.

```
+------------------+     +-----------------------+     +-----------------------------+
| 1. INPUT DATA    | --> | 2. PREPROCESSING      | --> | 3. ANALYSIS & ML            |
| (UAV, WV2, PS)   |     | (DOS1, Co-alignment)  |     | (ANOVA, Tukey HSD, PCA, RF) |
+------------------+     +-----------------------+     +-----------------------------+
```

---

## 📂 Repository Structure
```
├── 01_img_data/         # Multi-source raw & corrected raster datasets (UAV, WorldView-2, PlanetScope)
├── 03_pyscripts/        # Automated processing and analysis notebooks:
│   ├── EDA-1stA-SatByRes.ipynb     # Exploratory Data Analysis & statistical separability tests
│   └── PCSing-1stA-SatByRes.ipynb  # Radiometric correction, pansharpening, and feature extraction
├── 06_QGIS/             # Vector GIS boundary layers (vector patches & field limits)
├── 07_datasets/         # Extracted rodal-level average spectral data tables
├── 08_resultados/       # Output maps, plots, and figures
├── environment.yml      # Reproducible Conda environment specification
└── README.md            # Project documentation (this file)
```

---

## 📊 Key Scientific Insights
* **Spatial Resolution Impact:** Enhancing spatial resolution from 3.0 m (PlanetScope) to 0.5 m (WorldView-2 Pansharpened GS) reduces within-patch variance and edge-mixing effects, isolating pure pixels more effectively.
* **Spectral Sensitivity:** The Red, RedEdge, NIR, and NDVI channels show the highest sensitivity to chlorophyll absorption and structural canopy variation, making them key features for *S. halepense* identification.
* **The "Statistical Significance vs. Physical Magnitude" Paradox:** Large pixel-by-pixel sample sizes provide high statistical power (yielding highly significant ANOVA/Tukey $p < 0.001$ splits), though the physical reflectances of different weed densities remain extremely close. This highlights the need to integrate texture metrics (GLCM) and multitemporal fenology in subsequent modeling phases.

---

## ⚙️ Installation & Reproducibility

This project is packaged with a pre-configured Conda environment file containing all spatial library dependencies (`rasterio`, `geopandas`, `fiona`, `scikit-image`, etc.).

1. **Clone the repository:**
   ```bash
   git clone https://github.com/fjavierlopezc/vhr-satellite-weed-mapping.git
   cd vhr-satellite-weed-mapping
   ```

2. **Create the environment from the `environment.yml` file:**
   ```bash
   conda env create -f environment.yml
   ```

3. **Activate the environment:**
   ```bash
   conda activate esa-weed-mapping
   ```

4. **Launch Jupyter:**
   ```bash
   jupyter notebook
   ```

---

## 📧 Contact
For academic inquiries regarding methodology or data sharing:
* **F. Javier López-C.** - *ICA-CSIC (Instituto de Ciencias Agrarias - Consejo Superior de Investigaciones Científicas)*
* **Repository Host:** [fjavierlopezc/vhr-satellite-weed-mapping](https://github.com/fjavierlopezc/vhr-satellite-weed-mapping)
