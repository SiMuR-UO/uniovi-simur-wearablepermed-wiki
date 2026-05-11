---
layout: default
title: "BBB"
nav_order: 2
parent: "Technical Reports"
---

# Multisensor Classification
This study investigates multimodal human activity recognition (HAR) using wearable IMUs, focusing on fusion strategies that combine wrist and thigh data to improve classification.

The dataset includes 62 participants; signals were synchronized, corrected for drift, segmented into 10-second windows with 50% overlap, and used to classify activities at multiple granularities (4, 8, 15 classes).

Approaches compared:
- Single-sensor baselines (wrist-only, thigh-only)
- Early fusion (feature concatenation + Random Forest)
- Late fusion (stacking with Random Forest + Logistic Regression meta-model)
- Mixture-of-Experts (MoE) with Random Forest and autoencoder experts

### Key Findings

- Fusion improves performance: all fusion strategies outperform single-sensor models.
- Late fusion (stacking, MoE) consistently yields higher accuracy and stability than early fusion.
- Random Forest-based fusion methods (stacking, MoE) provide the best trade-off between performance and robustness.
- Autoencoder-based methods show higher variance and lower stability compared to tree-based ensembles.

### Key Insights & Conclusion

- Combining sensors provides richer representations and more robust predictions; decision-level (late) fusion is more scalable and effective than simple feature concatenation.
- Tree-based ensemble fusion offers a practical, stable solution for real-world HAR deployments.
