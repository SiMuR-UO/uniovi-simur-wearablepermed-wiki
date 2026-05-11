---
layout: default
title: "Monosensor vs. Dual-Sensor Human Activity Recognition"
nav_order: 2
parent: "Other Research Studies"
---

# Monosensor vs. Dual-Sensor Human Activity Recognition from public HAR datasets

This study explores the effectiveness of combining wearable sensors placed at different body locations (wrist and thigh) for human activity recognition using Deep Convolutional Neural Networks (CNNs).

## Motivation

The IMPaCT cohort protocol includes simultaneous wearable monitoring on both wrist and thigh, hypothesizing that dual-location placement would enhance activity classification accuracy compared to single-location monitoring.

## Methodology

### Datasets

- **Realistic Sensor Displacement (RSD):** 17 subjects, 50+ activities
- **Daily and Sport Activities (DSA):** 8 subjects, 50+ activities
- **Total:** ~50 distinct activities (walking, running, jumping, cycling, sports, exercises, postures, etc.)

### Preprocessing

- Data aligned to anatomical axes (X: anterior-posterior, Y: medial-lateral, Z: vertical)
- Resampled to 25 Hz
- Segmented into 2-second windows with 1-second overlap
- Total: 51,585 and 45,443 data windows from RSD and DSA respectively

### CNN Architecture

- Random Search optimization over 150 trials
- Three network configurations:
  - **NET_TW:** Thigh + Wrist (12 features)
  - **NET_T:** Thigh only (6 features)
  - **NET_W:** Wrist only (6 features)

## Key Findings

- **Dual-sensor (NET_TW):** 85% test accuracy
- **Thigh-only (NET_T):** 69% test accuracy
- **Wrist-only (NET_W):** 68% test accuracy
- **Improvement:** >15% accuracy gain when combining both sensors

Classification confusion analysis revealed that overlapping activities (e.g., basketball containing walking/running episodes, activity transitions within 2-second windows) are primary sources of misclassification, suggesting actual performance may be higher.

## Conclusion

Simultaneous dual-location wearable monitoring significantly enhances human activity recognition performance, supporting the IMPaCT cohort's protocol design. However, real-world conditions may differ from controlled experimental settings.

### Reference

Castellanos, A., López, A. M., García, D., Álvarez, D., & Álvarez, J. C. (2023). Human Activity Recognition from Thigh and Wrist Accelerometry. ESANN 2024 proceedings, European Symposium on Artificial Neural Networks, Computational Intelligence and Machine Learning. Bruges (Belgium) and online event, 9-11 October 2024, i6doc.com publ., ISBN 978-2-87587-090-2. [Paper available from this link 🔬](https://www.esann.org/sites/default/files/proceedings/2024/ES2024-75.pdf.)

### Disemination

*** Jornadas de dcotorado y otros ***