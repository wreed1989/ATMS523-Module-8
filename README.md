# Solar-Wind Drivers of Geomagnetic Storms (1999–2020):  
## Identifying Multi-Parameter Predictors of Global Geomagnetic Activity

---

## Motivation

As part of an operational role within the **557th Weather Wing – Space Weather Operations**, a major component of daily workflow involves assessing how upstream solar-wind conditions translate into geomagnetic storming that may impact DoD systems and mission planning.

While forecasting often relies on familiar indicators—such as **southward IMF Bz**, **solar-wind speed**, and the **arrival of CME-driven disturbances**—operational decisions typically depend on the combined behavior of multiple solar-wind parameters. In practice, the solar wind rarely presents clean, isolated signals. Fast wind with weak Bz can still drive elevated activity, dense shocks with moderate Bz can create strong compressions in the magnetosphere, and interactions among IMF magnitude (Bt), Bz polarity, dynamic pressure, and density frequently reinforce or counteract one another.

Motivated by these operational challenges, this project evaluates **how solar-wind variables jointly control geomagnetic-storm probability**, rather than examining each parameter independently. The goals of this analysis are to:

- Identify combinations of solar-wind conditions that elevate operational risk  
- Quantify which parameters exert the strongest influence on storm onset  
- Compare multi-parameter behavior with traditional single-variable thresholds  

The project implements an **end-to-end workflow** linking real solar-wind observations, storm classifications (Ap > 30 / 50 / 100), probability modeling, and interpretable machine-learning decision rules—providing the type of diagnostic insight relevant to operational space-weather assessments.

---

## Approach

To investigate how different aspects of the solar wind influence geomagnetic activity, a Jupyter Notebook was developed entirely in Python using standard packages like `pandas`, `numpy`, `scikit-learn`, and `matplotlib`. The workflow consists of the following stages:

### **Dataset Selection and Preparation**

The project uses **ACE Level-2 Solar-Wind Data** and standard **SWPC-processed solar-wind parameters**, including:

- IMF Bz in GSM Coordinates (nT)
- IMF magnitude Bt (nT)
- Solar-wind speed (km/s)
- Proton density (cm⁻³)

Geomagnetic activity was categorized using the **Ap index**, which provides a globally averaged disturbance measure consistent with the temporal resolution of the ACE and SWPC datasets:

- **Ap > 30** — Minor geomagnetic storm  
- **Ap > 50** — Moderate/strong storm  
- **Ap > 100** — Major storm  

### **Feature Engineering**

To capture storm-relevant physical processes, several derived features were calculated:

- **Dynamic pressure:**  
  \( P_{dyn} = 1.6726 \times 10^{-6} \, n \, V^2 \)  
- **vBz and vBz_neg:** speed-weighted coupling terms for testing reconnection efficiency  
- **Negative-only Bz:** isolating the geoeffective southward IMF component  
- **Lagged features (1–24 hours):** characterizing temporal evolution leading to storm onset  

### **Threshold and Probability Analysis**

The analysis examined storm likelihood across multiple dimensions:

- **Single-variable thresholds:** storm fraction across bins of Bz, Bt, speed, and density  
- **Pairwise probability surfaces:**  
  (Bz × Speed), (Bt × Speed), (Bz × Density), (Speed × Density), and others  
- **Multivariate decision rules:** interpretable thresholds from decision-tree classifiers  
- **Two-dimensional binned probability fields:** identifying high-probability storm regions  
- **SHAP-style model interpretability:** ranking the relative influence of each parameter  

These steps revealed that geomagnetic storms typically arise from combinations of solar-wind conditions rather than from any single variable acting alone.

### **Machine-Learning Modeling**

To evaluate nonlinear interactions and multi-parameter predictive skill, the project incorporated:

- **Decision tree classifiers** for interpretable threshold extraction  
- **Random forests** for capturing nonlinear interactions  
- **Probability calibration** to quantify storm likelihood  
- **SHAP analysis** to investigate feature importance  

This produced a multivariate representation of the solar-wind environments most likely to generate geomagnetic storms.

---

## Summary of Findings

Across all three storm categories (Ap > 30, Ap > 50, Ap > 100), several consistent patterns emerged:

- **IMF magnitude (Bt) and solar-wind speed are the dominant predictors**, exceeding the influence of Bz alone.  
- Storm probability increases sharply when:  
  - Bt > 10–12 nT  
  - Solar-wind speed > 550–600 km/s  
  - Density > 10–15 cm⁻³ (indicative of compression regions)  
- Strong negative Bz values were rare but consistently geoeffective.  
- Pairwise interactions—especially **Bt × Speed** and **Bz × Speed**—produced the strongest storm-probability signatures.  
- Multivariate decision-tree rules recovered realistic operational thresholds, indicating that a combination of fast wind, elevated IMF magnitude, and moderate southward Bz is often sufficient for storm development.  

Overall, the results quantify well-known operational intuition with statistical evidence derived from long-term datasets.

---

## Data Sources

### **ACE Solar-Wind Data (SWEPAM, 1-Hour Level-2)**
Caltech / ACE Science Center – Level-2 SWEPAM 1-hour dataset  
https://izw1.caltech.edu/cgi-bin/dib/rundibviewswel2/ACE/ASC/DATA/level2/swepam?swepam_data_1hr.hdf!hdfref;tag=1962,ref=3,s=0

### **ACE Magnetic-Field Data (MAG, 1-Hour Level-2)**
Caltech / ACE Science Center – Level-2 MAG 1-hour dataset  
https://izw1.caltech.edu/cgi-bin/dib/rundibviewmagl2/ACE/ASC/DATA/level2/mag?mag_data_1hr.hdf!hdfref;tag=1962,ref=6,s=0

### **Ap & Kp Geomagnetic Indices**
GFZ Potsdam – Official Kp/Ap data archive  
https://kp.gfz.de/en/data  

The Ap and Kp datasets are not included in this repository due to file-size limitations.

---
