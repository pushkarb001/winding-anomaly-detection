# Winding Process Anomaly Detection

![Python](https://img.shields.io/badge/Python-3.12-blue)
![scikit-learn](https://img.shields.io/badge/scikit--learn-1.8-orange)
![Status](https://img.shields.io/badge/Status-Complete-brightgreen)

Unsupervised anomaly detection system for PMSM motor winding process signals. Detects abnormal winding cycles from machine signal patterns — without requiring labelled failure data. Combines ML-based detection (Isolation Forest, LOF) with classical Statistical Process Control (Shewhart, CUSUM, EWMA charts).

---

## Project Motivation

In manufacturing quality monitoring, labelled failure data is rare and expensive to collect. This project demonstrates that a complete anomaly detection system can be built using **only normal operating data** — the model learns what normal looks like, then flags anything that deviates.

This directly addresses the inline monitoring requirement in joining technology research: detect quality deviations during the process before defective parts proceed downstream.

---

## Dataset

500 synthetic winding cycles with 5 injected anomaly types:

| Anomaly Type | Physical Meaning | Primary Signal |
|---|---|---|
| `tension_spike` | Wire snag / sudden resistance | tension_std_N, tension_peak_N |
| `overheating` | Insulation thermal damage | temp_max_C, temp_gradient |
| `power_surge` | Spindle overload | power_mean_W, power_std_W |
| `speed_instability` | Drive controller fault | winding_speed_rpm, power_std_W |
| `combined_stress` | Multiple simultaneous faults | Multiple signals |

---

## Methodology

```
Normal cycles (400 samples)
        ↓
Baseline statistics computed (mean, std per signal)
        ↓
Scaler fitted on normal data only — no data leakage
        ↓
Three unsupervised models trained on normal data:
  • Statistical z-score baseline
  • Isolation Forest
  • Local Outlier Factor
        ↓
Three SPC charts on simulated production run:
  • Shewhart (3-sigma) — sudden shifts
  • CUSUM — sustained small drifts
  • EWMA — gradual trends
        ↓
Combined detection system
```

---

## Results

| Model | AUC | F1 | Precision | Recall |
|---|---|---|---|---|
| Z-score baseline | — | — | — | — |
| Isolation Forest | >0.85 | >0.75 | — | — |
| Local Outlier Factor | >0.80 | >0.70 | — | — |

---

## Repository Structure

```
winding-anomaly-detection/
├── data/raw/
│   └── winding_anomaly_data.csv
├── notebooks/
│   ├── 01_EDA.ipynb
│   ├── 02_AnomalyDetection.ipynb
│   └── 03_ControlCharts.ipynb
├── src/
│   ├── anomaly_scaler.pkl
│   ├── isolation_forest.pkl
│   └── lof_model.pkl
├── results/plots/
└── README.md
```

---

## How to Run

```bash
git clone https://github.com/pushkarb001/winding-anomaly-detection.git
cd winding-anomaly-detection
pip install -r requirements.txt
```

Run notebooks in order: 01 → 02 → 03

---

## Relation to PMSM Quality Prediction Project

This project is a companion to the [PMSM Winding Quality Prediction](https://github.com/pushkarb001/pmsm-winding-quality-prediction) project. Where that project uses supervised learning to predict quality outcomes from labelled data, this project uses unsupervised learning to detect anomalies without any labels — representing the realistic production scenario where failure labels are unavailable.

Together the two projects cover the full spectrum of data-driven process monitoring methodology.
