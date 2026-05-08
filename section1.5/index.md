---
layout: default
title: "Section 1.5"
nav_order: 3
has_children: true
---

# Introducción

Esta sección describe los principales estudios realizados.

## MATRIX Evaluation

This work evaluates the capability of **magnetometer-less IMUs** for estimating 3D orientation, with a particular focus on comparing **open-source solutions against commercial devices**, including the **MATRIX IMU (s-II)**.

The study investigates whether a **custom-built IMU + open-source sensor fusion algorithm** can achieve performance comparable not only to high-end proprietary systems (e.g., Xsens), but also to **commercial low-cost platforms such as MATRIX**, which are commonly used in wearable sensing applications.

### Methodology

Three sensor configurations were compared:
- **Proprietary reference (s-I)**: Xsens IMU with built-in Kalman-based filter  
- **Commercial low-cost (s-II)**: MATRIX IMU + open-source complementary filter  
- **Open-source system (s-III)**: custom-built IMU (bIMU) + complementary filter

Experiments were conducted using:
- A **robotic arm providing ground-truth orientation**  
- Controlled **single-axis rotations in 5° steps**  
- Repeated trials for statistical robustness  

All non-proprietary systems (including MATRIX) used the same:
- **Accelerometer + gyroscope signals (no magnetometer)**  
- **Complementary filter (CF)** for fair comparison 


Performance metrics:
- Orientation error  
- Variability (STD) in static and dynamic conditions  
- Sensor noise characterization  

### Key Findings
- **Open system vs MATRIX**:    The custom IMU (OPEN) achieves lower error (~2.10%, ~0.11°) and better repeatability than the MATRIX-based configuration (S‑B), highlighting the impact of sensor quality. 
- **MATRIX performance limitations**:    The MATRIX IMU (s-II) shows the **highest error (~2.88%) and variability**, mainly due to higher sensor noise compared to the other devices. 
- **Sensitivity to sensor noise**:    Differences between systems are primarily explained by **intrinsic noise characteristics**, with MATRIX presenting higher noise levels due to cost-optimized components. 
- **Algorithm vs hardware**:    Using the same complementary filter, performance differences between systems (including MATRIX) are mainly driven by **hardware quality rather than algorithm choice**. 
- **Comparison with proprietary systems**:    Despite MATRIX limitations, all systems remain within a narrow error range (<3%), showing that even low-cost devices can provide usable orientation estimates. 

### Key Insights
- The **MATRIX IMU provides a practical baseline** for wearable sensing:  
    - Low-cost and deployable    
    - Acceptable accuracy for many applications  
- However, compared to MATRIX:  
    - **Higher-quality sensors significantly reduce error and variability**    
    - **Sensor noise becomes the dominant limiting factor**  
    - Open-source solutions can:  
        - Match or outperform MATRIX    
        - Offer **greater flexibility and transparency**  
### Conclusion

This work demonstrates that while the **MATRIX IMU represents a strong low-cost commercial baseline**, its performance is constrained by higher sensor noise levels. In contrast, **custom open-source IMUs combined with simple fusion algorithms can outperform MATRIX and approach high-end commercial systems**, highlighting the importance of sensor quality in orientation estimation.

These findings provide practical guidance for choosing between:- **Cost-efficient solutions (MATRIX)**  - **High-performance open or proprietary alternatives**

### Dissemination

This study has been published in [IEEE Transactions on Instrumentation and Measurement](https://ieeexplore.ieee.org/stamp/stamp.jsp?arnumber=11421575):
Title: Evaluation of Magnetometer-Less Inertial Sensors for Static and Dynamic Spatial Orientation Estimation
Authors: Alejandro Castellanos Alonso, Gonzalo García Carro, Juan C. Álvarez, Diego Álvarez Prieto, Antonio M. López Rodríguez, Diego Valdés Tirado

## Monosensor Classification

This work evaluates **human activity recognition (HAR)** using single wearable inertial sensors, focusing on the impact of **sensor placement, gyroscope integration, classification granularity, and model choice**.

The study is based on a dataset aligned with the IMPaCT cohort design, including data from 85 participants wearing sensors on the **wrist, thigh, and hip** during a set of controlled daily activities.

Two classification tasks are considered:
- **Fine-grained recognition** of 15 specific activities 
- **Coarse-grained classification** into 4 activity intensity levels (based on METs Compendium)

### Methodology

Sensor data (accelerometer and gyroscope) were:
- Corrected for timestamp drift and aligned with activity labels 
- Segmented into 10-second windows with 50% overlap 
- Used as input to multiple models:
    - Convolutional Neural Networks (CNNs) 
    - Random Forest 
    - XGBoost  

Both **accelerometer-only** and **accelerometer + gyroscope** configurations were evaluated across all sensor locations.

### Key Findings

- **Classification granularity is the most important factor**:    Coarse-grained (intensity-level) classification clearly outperforms fine-grained activity recognition. [1](https://unioviedo-my.sharepoint.com/personal/amlopez_uniovi_es/_layouts/15/Doc.aspx?sourcedoc=%7B5CAD84FD-02F4-4E84-B9DF-57CED6C6A580%7D&file=Paper_Monosensor_Classification_WearablePerMed.docx&action=default&mobileredirect=true) 
- **Sensor placement matters**:
    - Wrist performs best for fine-grained activity recognition
    - Thigh performs best for intensity-level classification [1](https://unioviedo-my.sharepoint.com/personal/amlopez_uniovi_es/_layouts/15/Doc.aspx?sourcedoc=%7B5CAD84FD-02F4-4E84-B9DF-57CED6C6A580%7D&file=Paper_Monosensor_Classification_WearablePerMed.docx&action=default&mobileredirect=true) 

- **Gyroscope improves performance**:    Adding angular velocity data consistently increases accuracy across models and configurations, contributing ~40% of relevant features. [1](https://unioviedo-my.sharepoint.com/personal/amlopez_uniovi_es/_layouts/15/Doc.aspx?sourcedoc=%7B5CAD84FD-02F4-4E84-B9DF-57CED6C6A580%7D&file=Paper_Monosensor_Classification_WearablePerMed.docx&action=default&mobileredirect=true)
- **Model differences are secondary**:    Random Forest, XGBoost, and CNNs achieve comparable results, with no major practical differences compared to other factors. 

### Key Insights
- Fine-grained activity recognition is inherently difficult due to **similar motion patterns between activities** (e.g., different types of walking).
- Grouping activities into **intensity-based categories** provides a more robust and reliable classification framework. 
- The inclusion of gyroscope data improves performance, but may introduce trade-offs in **cost, complexity, and energy consumption**. 

### Conclusion

Single-sensor HAR systems can achieve strong performance, but results are highly dependent on **how the problem is defined (classification granularity)** and **which sensor configuration is used**. Incorporating gyroscope data and carefully selecting sensor placement can provide **moderate but consistent improvements**, while intensity-based classification offers a more scalable and robust alternative for large population studies.

### Dissemination

This study has been submitted for publication to Sensors:

**Title**: Impact of Gyroscope Integration, Sensor Placement, and Activity
Granularity on Human Activity Recognition Performance

**Authors**: Alejandro Castellanos, Antonio M. López, Miguel Á. Salinas, Juan C.
Álvarez, Diego Álvarez, Gonzalo García, Ángel Buendía-Romero, Asier
Mañas, Raquel Bailón, Vicente Martín, Ana Carbonell Baeza, Verónica
Cabanas-Sánchez, David Martinez-Gomez

## Multisensor Classificacion

This work focuses on **multimodal human activity recognition (HAR)** using wearable inertial sensors, exploring how different **fusion strategies** can improve classification performance.

Instead of relying on a single sensor, the study combines data from **wrist and thigh IMUs** to exploit their complementary information. 

Multiple fusion approaches are evaluated to determine how best to integrate heterogeneous sensor data for activity recognition tasks. 

### Methodology

The dataset includes IMU data from **62 participants**, collected with wrist and thigh sensors over multiple days. Signals were:
- Synchronized and corrected for timestamp drift 
- Segmented into 10-second windows with 50% overlap 
- Used to classify activities at different levels of granularity (4, 8, and 15 classes) 

Three types of approaches were evaluated:
- Single-sensor baseline- Wrist-only model  - Thigh-only model  
- Early fusion
    - **Feature concatenation** followed by a Random Forest classifier  
- Late fusion
    - **Stacking** (Random Forest + Logistic Regression meta-model)  
    - **Mixture of Experts (MoE)** with:  
        - Random Forest experts    
        - Autoencoder experts    
        - Variational autoencoder (VAE) experts  Performance was evaluated using the **F1-score**. 
        
### Key Findings
- **Fusion improves performance**:    All fusion strategies outperform single-sensor models, confirming the complementarity of wrist and thigh data. 

- **Late fusion is the most effective**:    Stacking and MoE approaches consistently achieve higher accuracy than simple feature concatenation. 

- **Best models: Random Forest-based fusion**    Stacking (RF) and MoE (RF) provide the highest performance and stability across all scenarios. 
- **Performance decreases with granularity**:    Recognizing more detailed activity classes reduces accuracy, but fusion methods remain advantageous.
- **Autoencoder-based methods are less stable**:    They show higher variance and lower performance compared to tree-based approaches. 
### Key Insights
- Combining sensors provides **richer and more robust representations** of human activity 
- **Decision-level fusion (late fusion)** is more scalable and effective than feature-level fusion 
- Tree-based ensemble models offer the best balance between **performance, robustness, and simplicity**  
### Conclusion
Multimodal fusion of wearable sensors significantly improves HAR performance compared to single-sensor approaches.

**Late fusion strategies**, particularly stacking and Mixture of Experts with Random Forest, provide the most reliable results, especially in complex classification tasks.

These findings support the use of **multisensor wearable systems** in large-scale and real-world applications, offering practical guidelines for designing robust activity recognition pipelines.

## MET estimation

This work focuses on the estimation of **physical activity intensity in terms of metabolic equivalents (METs)** using wearable inertial sensors, as an alternative to traditional activity classification approaches.

Using data from the WearablePerMed project (aligned with the Spanish IMPaCT cohort), the study evaluates how well **machine learning and deep learning models** can predict continuous energy expenditure from IMU signals collected at the **wrist, thigh, and combined configurations**.

### Methodology

The dataset includes both:
- **Structured activities** with ground-truth METs measured via indirect calorimetry 
- **Semi-structured daily activities** annotated using reference MET values  

Sensor data (accelerometer + gyroscope) were:
- Corrected for timestamp drift and synchronized 
- Segmented into 10-second windows with 50% overlap 
- Transformed into feature-based and raw time-series representations 

Two modeling approaches were evaluated:
- **Feature-based models**: Ridge Regression, Random Forest, XGBoost, Feed-Forward NN 
- **Time-series models**: CNN, LSTM, Transformer 

Performance was assessed using:
- **RMSE (Root Mean Squared Error)**
- **R² (coefficient of determination)**

### Key Findings

- **Ensemble methods perform best**:    XGBoost and Random Forest achieve the lowest error and highest R² values across configurations.
- **Multisensor improves performance**:    Combining wrist + thigh consistently reduces prediction error compared to single-sensor setups.
 - **Single sensors are still competitive**:    Wrist-only and thigh-only configurations achieve strong performance, suggesting viable lower-cost alternatives. 
  - **Deep learning models are competitive but not dominant**:    CNN and LSTM models perform well, but do not consistently outperform classical ensemble methods.
### Key Insights
- Estimating METs provides a **continuous, physiologically meaningful representation of activity**, avoiding the limitations of discrete activity classification. 
- Sensor placement and configuration remain critical design choices for large-scale monitoring systems. 
- Ensemble-based models offer a strong balance of **accuracy, robustness, and interpretability** for energy expenditure estimation. 
  
  ### Conclusion
  
  This work demonstrates that **MET estimation from wearable IMUs is a feasible and scalable alternative to activity classification** for population-level studies.
  
  While dual-sensor configurations provide the best performance, **single-sensor setups can still achieve accurate predictions**, enabling more cost-effective deployments.
  
  The results support the use of **machine learning–based MET estimation pipelines** in large-scale cohort studies and real-world monitoring applications.

## Semi-supervised classification

This work explores **human activity recognition (HAR)** using a **single wearable inertial sensor**, incorporating both **labeled laboratory data and unlabeled free-living data** from the WearablePerMed dataset.

The approach is based on **semi-supervised learning**, aiming to leverage large amounts of unlabeled data collected during real-world conditions to improve classification performance while reducing the need for manual annotation.

This study follows the WearablePerMed protocol, which includes:- **Structured and semi-structured laboratory activities**
- **7-day free-living recordings** capturing natural daily behavior

### Methodology

The dataset is divided into three subsets:
- **Labeled set (L)**: annotated laboratory activities 
- **Unlabeled set (U)**: large volume of free-living data 
- **Test set (T)**: held-out labeled data for evaluation 

 The proposed pipeline consists of several stages:
 - Representation learning
    - A **1D convolutional autoencoder** is trained on both labeled and unlabeled data.The encoder learns compact representations of movement without requiring labels 
    - Supervised baseline
        -  A classifier (encoder + dense layer) is trained using only labeled data  
    - Semi-supervised learning strategies
        - **Self-training**: pseudo-labeling high-confidence predictions (>0.95)  
        - **Self-Adaptive Thresholding (SAT)**: dynamic confidence thresholds per class  
        - **FixMatch**: consistency-based learning using weak and strong augmentations 
        
    All models operate under a **single-sensor setup** (e.g., wrist or thigh).
    
### Expected Findings
- **Leveraging unlabeled data improves performance**:    Free-living data provides additional variability that can enhance model generalization. 
- **Representation learning is critical**:    Autoencoders help capture underlying motion patterns before classification. 
 - **Semi-supervised methods increase robustness**:    Techniques like FixMatch enforce consistency and improve performance under noisy or heterogeneous conditions. 
 - **Single-sensor setups remain practical**:    The approach aligns with secondary objectives of WearablePerMed to support configurations with missing sensors. [1](https://unioviedo-my.sharepoint.com/personal/amlopez_uniovi_es/Documents/Archivos%20de%20Microsoft%C2%A0Copilot%20Chat/Study%20Protocols.pdf)  
### Key Insights
- Free-living data introduces **real-world variability** not present in controlled laboratory settings  
- Semi-supervised learning helps bridge the gap between **controlled and natural environments**  
- Reducing dependency on annotations is essential for **scalable population-level studies**  

## Image Annotation