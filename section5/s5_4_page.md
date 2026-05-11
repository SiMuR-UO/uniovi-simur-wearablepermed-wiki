---
layout: default
title: "MET Estimation"
nav_order: 4
parent: "Dataset Research Studies"
---

# MET Estimation
This work focuses on estimating physical activity intensity in terms of metabolic equivalents (METs) using wearable IMU signals, as an alternative to activity classification.

The dataset includes structured activities with indirect calorimetry ground truth and semi-structured daily activities annotated with reference METs. Sensor configurations tested include wrist-only, thigh-only, and combined wrist+thigh.

### Methodology

Signals were corrected, synchronized, segmented into 10-second windows with overlap, and represented using both engineered features and raw time-series. Models evaluated included feature-based ensembles (Ridge, Random Forest, XGBoost) and time-series models (CNN, LSTM, Transformer). Performance was measured using RMSE and R².

### Key Findings

- Ensemble methods (XGBoost, Random Forest) achieve the lowest error and highest R² values.
- Multisensor (wrist+thigh) configurations reduce prediction error compared to single sensors.
- Deep learning models (CNN, LSTM) are competitive but do not consistently outperform ensemble methods.

### Key Insights & Conclusion

- MET estimation provides a continuous, physiologically meaningful representation of activity and is suitable for population-level studies.
- Ensemble-based models and multisensor configurations offer the best balance of accuracy, robustness, and interpretability.
