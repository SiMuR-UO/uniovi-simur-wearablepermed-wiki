---
layout: default
title: "Unsupervised Visualization"
nav_order: 7
parent: "Dataset Research Studies"
---


This work focuses on **semi-supervised labeling of human activity data** using an **interactive data visualization approach**, applied to the WearablePerMed dataset.

Unlike traditional fully automated methods, the proposed approach combines **dimensionality reduction, visualization, and human interaction** to facilitate efficient and interpretable labeling of physical activity data, particularly in **free-living conditions** where annotations are scarce and complex. [1](https://unioviedo-my.sharepoint.com/personal/amlopez_uniovi_es/Documents/Archivos%20de%20Microsoft%C2%A0Copilot%20Chat/PaperVisualizacion.pdf) 

---

# Methodology

The approach is applied to data from WearablePerMed, including:
- **Continuous 7-day free-living recordings**
- Accelerometer data from **wrist and thigh sensors (MATRIX IMUs)** [1](https://unioviedo-my.sharepoint.com/personal/amlopez_uniovi_es/Documents/Archivos%20de%20Microsoft%C2%A0Copilot%20Chat/PaperVisualizacion.pdf) 

## Data processing
- Signals segmented into **30-second windows**.
- Feature extraction based on:
  - Time-domain statistics (mean, percentiles, etc.).
  - ENMO-based activity descriptors. [1](https://unioviedo-my.sharepoint.com/personal/amlopez_uniovi_es/Documents/Archivos%20de%20Microsoft%C2%A0Copilot%20Chat/PaperVisualizacion.pdf) 

## Visualization framework

The core contribution is an **interactive visual analytics system** combining:

- **Dimensionality reduction techniques**:
  - PCA (global structure).
  - t-SNE and UMAP (local clustering).
  - Autoencoders (latent representations).

- **Morphing Projections**:
  - Dynamic combination of multiple projections into a single view.

- **Linked Selections**:
  - Synchronization between:
    - Projection space.
    - Temporal evolution.
    - Raw signals and wearable camera data.

- **Hierarchical clustering (MetaClusters)**:
  - Used as a basis for grouping similar activity patterns.

This system allows users to explore and label data visually and interactively. [1](https://unioviedo-my.sharepoint.com/personal/amlopez_uniovi_es/Documents/Archivos%20de%20Microsoft%C2%A0Copilot%20Chat/PaperVisualizacion.pdf) 

---

# Key Findings

- **High labeling accuracy with human-in-the-loop**: 
The method achieves **95% correct and interpretable labeling** of activity data. [1](https://unioviedo-my.sharepoint.com/personal/amlopez_uniovi_es/Documents/Archivos%20de%20Microsoft%C2%A0Copilot%20Chat/PaperVisualizacion.pdf) 

- **Effective for free-living data**: 
Successfully applied to real-world, unconstrained activity data collected over 7 days. [1](https://unioviedo-my.sharepoint.com/personal/amlopez_uniovi_es/Documents/Archivos%20de%20Microsoft%C2%A0Copilot%20Chat/PaperVisualizacion.pdf) 



- **Reduced need for manual annotation**: 
The approach significantly lowers labeling effort compared to fully manual methods. [1](https://unioviedo-my.sharepoint.com/personal/amlopez_uniovi_es/Documents/Archivos%20de%20Microsoft%C2%A0Copilot%20Chat/PaperVisualizacion.pdf) 

- **Different projections provide complementary insights**: 
Combining PCA, UMAP, t-SNE, and autoencoders enhances pattern discovery. [1](https://unioviedo-my.sharepoint.com/personal/amlopez_uniovi_es/Documents/Archivos%20de%20Microsoft%C2%A0Copilot%20Chat/PaperVisualizacion.pdf) 

---

# Key Insights

- Interactive visualization enables:
  - **Explainable labeling decisions**.
  - Detection of **complex and nonlinear activity patterns**.

- Human-guided semi-supervised learning:
  - Avoids dependence on fully labeled datasets.
  - Improves robustness in **high-variability environments**.

- Compared to automatic approaches:
  - More interpretable.
  - Better suited for **non-structured real-world data**.

---

# Limitations

- Assumes **constant activity within 30-second windows**, which may ignore transitions.
- Depends on **human interaction**, limiting scalability.
- Slower than fully automated methods once models are trained.

---

# Conclusion

This work introduces an innovative **visual analytics framework for semi-supervised labeling** of wearable sensor data.

By combining **interactive visualization, dimensionality reduction, and human expertise**, the approach enables accurate and explainable labeling in **free-living conditions**, bridging the gap between manual annotation and fully automated machine learning.

It represents a promising direction for **scalable and interpretable activity recognition pipelines** in large population studies.

## Report

<figure>
  <iframe src="./assets/files/Visualizacion.pdf" width="320" height="240" style="width:320px;max-width:100%;height:240px;" title="Visualizacion.pdf">
      This browser does not support PDFs. Please download the PDF to view it: <a href="./assets/files/poster_fusion.pdf">Download PDF</a>
  </iframe>
</figure>
