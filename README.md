# Solar-Wind Drivers of Geomagnetic Storms (1999–2025):  
## Identifying Multi-Parameter Predictors of Global Geomagnetic Activity

---

## Motivation

As part of my operational role in the **557th Weather Wing – Space Weather Operations**, a major part of the workflow involves assessing how upstream solar-wind conditions translate into geomagnetic storming which may impact DoD systems and operations.

While day-to-day forecasting often relies on well-known indicators—such as **southward IMF Bz**, **solar-wind speed**, and the **arrival of CME-driven enhancements**—operational decisions typically depend on the *combined* behavior of these parameters. In practice, the solar wind rarely presents clean, isolated signals: fast wind with weak Bz can still drive disturbances, dense shocks with moderate Bz can create sharp compressions in the magnetotail and IMF magnitude (Bt), Bz polarity, dynamic pressure, and density often reinforce or compete with each other.

Because of this, I wanted to build a project that helps evaluate **how different solar-wind variables jointly control geomagnetic-storm probability**, not just individually. I intend to:

- Identify combinations of solar-wind conditions that elevate operational risk,  
- Quantify which parameters matter most for storm onset
- Evaluate how multi-parameter behavior compares to simple thresholds

This project therefore focuses on an **end-to-end workflow** linking real solar-wind data, storm labels (Ap > 30 / 50 / 100), probability models, and interpretable machine-learning decision rules—mirroring how storm guidance might be operationally assessed in real time.

---

## Approach

To explore how different aspects of the solar wind drive geomagnetic variability, I constructed an analysis pipeline built entirely in Python using `pandas`, `numpy`, `scikit-learn`, and `matplotlib`. The workflow consists of the following steps:

### **Dataset Selection and Preparation**

I used **ACE Level-2 Real-Time Solar Wind** and standard **SWPC-processed solar-wind parameters**, including:

- IMF Bz (GSM)
- IMF magnitude Bt
- Solar-wind speed (km/s)
- Proton density (cm⁻³)

Storm labels were defined using the **Ap index**, since Ap provides globally averaged information consistent with the time resolution of the ACE and SWPC solar-wind data:

- **Ap > 30** — Minor geomagnetic storm  
- **Ap > 50** — Moderate/strong storm  
- **Ap > 100** — Major storm  

### **Feature Engineering**

To capture storm-relevant physical behavior, I created:

- **Dynamic Pressure:**  
  \( P_{dyn} = 1.6726 \times 10^{-6} \, n \, V^2 \)
- **vBz and vBz_neg:** speed-weighted coupling terms to test coupling of solar wind speed and magnetic orientation
- **Negative-only Bz:** since only southward Bz contributes to reconnection  
- **Lagged features (1–24 hours):** to capture temporal characteristics of storm onset

### **Threshold & Probability Analysis**

I computed **single-variable**, **pairwise**, and **multivariate** thresholds:

- **Single-variable bins**: How storm fraction changes across Bz, Bt, Speed, Density  
- **Pairwise binned probabilities**:  
  (Bz × Speed), (Bt × Speed), (Bz × Density), (Speed × Density), etc.  
- **Multivariate decision rules** using decision-tree classifiers  
- **Storm probability surfaces** using 2D binned statistics  
- **SHAP-style importance analysis** for interpretable ML models

These steps reveal how storms emerge not from a single solar-wind component, but from physical interactions (e.g., strong Bt + fast wind, or dense solar wind regimes + moderate southward Bz).

### **Machine-Learning Modeling**

To evaluate multi-parameter predictive power, I used:

- **Decision tree classifiers** (interpretable thresholds)  
- **Random forests** (nonlinear interactions, robust rankings)  
- **Probability calibration** for storm likelihood  
- **SHAP analysis** to evaluate relative feature importance  

The result is a multivariate understanding of what combinations of solar-wind parameters produce the highest storm probabilities.

---

## Summary of Findings

Across all three storm categories (Ap > 30 / 50 / 100), the analysis consistently showed:

- **Bt (IMF magnitude) and Solar-Wind Speed are the dominant factors**, not Bz alone.
- Storm probability escalates dramatically when:  
  - Bt > 10–12 nT  
  - Speed > 550–600 km/s  
  - Density > 10–15 cm⁻³ (compression effects)  
- Extreme negative Bz values were rare but always storm-producing.
- Pairwise interactions—especially **Bt × Speed** and **Bz × Speed**—were the strongest predictors.
- Multivariate decision-tree rules revealed realistic space-weather thresholds (fast wind + strong IMF + moderate Bz usually sufficient).

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

This aligns well with operational expectations but quantifies the relationships with actual statistics rather than intuition or isolated-case reasoning.

---
