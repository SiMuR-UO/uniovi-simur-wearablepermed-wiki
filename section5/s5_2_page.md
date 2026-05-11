---
layout: default
title: "Monosensor Classification"
nav_order: 2
parent: "Section 5"
---

# Monosensor Classification
This work evaluates human activity recognition (HAR) using single wearable inertial sensors, focusing on sensor placement, gyroscope inclusion, classification granularity, and model selection.

The dataset aligns with the IMPaCT cohort and includes data from ~85 participants wearing sensors on wrist, thigh, and hip during controlled activities. Two main tasks were studied: fine-grained recognition (15 activities) and coarse-grained intensity classification (4 classes based on METs).

### Methodology

Sensor signals (accelerometer and gyroscope) were corrected for timestamp drift, aligned with labels, segmented into 10-second windows with 50% overlap, and used as input to multiple models: CNNs, Random Forest, and XGBoost. Both accelerometer-only and accelerometer+gyroscope configurations were evaluated across sensor locations.

### Key Findings

- Classification granularity is the dominant factor: coarse-grained intensity classification clearly outperforms fine-grained activity recognition.
- Sensor placement matters: wrist yields best results for fine-grained tasks; thigh performs best for intensity-level classification.
- Gyroscope integration consistently improves accuracy and contributes a large fraction of relevant features (~40%).
- Model differences are secondary: Random Forest, XGBoost, and CNNs provide comparable practical results when other factors are controlled.

### Key Insights

- Fine-grained HAR is challenging due to similar motion patterns across activities; intensity-based grouping improves robustness.
- Including gyroscope data improves performance but may increase cost, complexity, and energy consumption.

### Conclusion & Dissemination

Single-sensor HAR systems can achieve strong performance provided the problem is well defined and the sensor configuration is chosen appropriately. This study has been submitted for publication (Sensors): "Impact of Gyroscope Integration, Sensor Placement, and Activity Granularity on Human Activity Recognition Performance." Authors: Alejandro Castellanos et al.
