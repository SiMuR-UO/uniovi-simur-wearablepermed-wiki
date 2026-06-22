---
layout: default
title: "Monosensor Classification"
nav_order: 2
parent: "Dataset Research Studies"
---

# Monosensor Classification
This work evaluates human activity recognition (HAR) using single wearable inertial sensors, focusing on sensor placement, gyroscope inclusion, classification granularity, and model selection.

The dataset aligns with the IMPaCT cohort and includes data from ~85 participants wearing sensors on wrist, thigh, and hip during controlled activities. Two main tasks were studied: fine-grained recognition (15 activities) and coarse-grained intensity classification (4 classes based on METs).

The work follows a preliminary study  using public HAR datasets presented at [ESANN](https://www.esann.org/sites/default/files/proceedings/2024/ES2024-75.pdf).

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

Single-sensor HAR systems can achieve strong performance provided the problem is well defined and the sensor configuration is chosen appropriately.

### Reference

Castellanos, A.; López, A.M.; Salinas, M.Á.; Álvarez, J.C.; Álvarez, D.; García, G.; Buendía-Romero, Á.; Mañas, A.; Bailón, R.; Martín, V.; et al. Impact of Gyroscope Integration, Sensor Placement, and Activity Granularity on Human Activity Recognition Performance. Sensors 2026, 26, 3683. [https://doi.org/10.3390/s26123683](https://doi.org/10.3390/s26123683)

Castellanos, A.; López, A.M.; García, D.; Álvarez, D.; Álvarez, J.C. Human Activity Recognition from Thigh and Wrist Accelerometry. In Proceedings of the 32nd European Symposium on Artificial Neural Networks, Computational Intelligence and Machine Learning ([ESANN 2024](https://www.esann.org/sites/default/files/proceedings/2024/ES2024-75.pdf)); i6doc: Louvain-la-Neuve, Belgium, 2024.

## Dissemination

<figure>
  <iframe src="./assets/files/2024_1_JornadasDoctoralesUniversidadOviedo.pdf" width="100%" height="600px" title="Poster de clasificación monosensor">
      This browser does not support PDFs. Please download the PDF to view it: <a href="./assets/files/2024_1_Joranadas Doctorales Universidad de Oviedo.pdf">Download PDF</a>
  </iframe>
</figure>

<figure>
  <iframe src="./assets/files/ES2024_WED_16_36_75_Alejandro_Castellanos_poster.pdf" width="100%" height="600px" title="ESANN 2024 Poster">
      This browser does not support PDFs. Please download the PDF to view it: <a href="./assets/files/2024_1_Joranadas Doctorales Universidad de Oviedo.pdf">Download PDF</a>
  </iframe>
</figure>