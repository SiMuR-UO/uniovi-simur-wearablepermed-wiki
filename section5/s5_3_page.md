---
layout: default
title: "Multisensor Classification"
nav_order: 3
parent: "Dataset Research Studies"
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

### Reference

- **The publication of a scientific article is planned for the recent future.**

## Dissemination

<figure>
  <figcaption>Jornadas de Doctorado del Departamento de Ingeniería Eléctrica, Electrónica, de Comunicaciones y de Sistemas de la Universidad de Oviedo, 2026.</figcaption>
  <iframe src="./assets/files/2024_jornadas_doctorales_uniovi_2.pdf" width="320" height="240" style="width:320px;max-width:100%;height:240px;" title="Poster de fusión multimodal">
      This browser does not support PDFs. Please download the PDF to view it: <a href="./assets/files/poster_fusion.pdf">Download PDF</a>
  </iframe>
</figure>