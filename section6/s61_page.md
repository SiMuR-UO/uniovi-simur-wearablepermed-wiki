---
layout: default
title: "Matrix Evaluation"
nav_order: 1
parent: "Other Research Studies"
---

# MATRIX Evaluation
This work evaluates the capability of magnetometer-less IMUs for estimating 3D orientation, with a focus on comparing open-source solutions against commercial devices, including the MATRIX IMU.

The study investigates whether a custom-built IMU combined with open-source sensor-fusion algorithms can reach performance comparable to high-end proprietary systems (e.g., Xsens) and to commercial low-cost platforms such as MATRIX.

### Methodology

Three sensor configurations were compared:
- Proprietary reference (s-I): Xsens IMU with built-in Kalman-based filter
- Commercial low-cost (s-II): MATRIX IMU with an open-source complementary filter
- Open-source system (s-III): custom-built IMU (bIMU) with complementary filter

Experiments used a robotic arm providing ground-truth orientation, controlled single-axis rotations in 5° steps, and repeated trials for statistical robustness. All non-proprietary systems used accelerometer + gyroscope signals (no magnetometer) and the same complementary filter to allow a fair comparison.

Performance metrics included orientation error, variability (STD) under static and dynamic conditions, and sensor noise characterization.

### Key Findings

- Open system vs MATRIX: the custom IMU achieves lower error (~2.10%, ~0.11°) and better repeatability than the MATRIX-based configuration, emphasising sensor quality impact.
- MATRIX limitations: the MATRIX IMU shows the highest error (~2.88%) and variability, mainly due to higher sensor noise from cost-optimised components.
- Sensitivity to sensor noise: differences between systems are primarily explained by intrinsic noise characteristics rather than algorithmic choices.
- Algorithm vs hardware: with the same complementary filter, hardware quality dominates performance differences.
- Comparison with proprietary systems: despite limitations, all systems remain within a narrow error range (<3%), indicating low-cost devices can still provide usable orientation estimates for many applications.

### Key Insights

- The MATRIX IMU provides a practical low-cost baseline: deployable and acceptable accuracy for many use cases.
- Higher-quality sensors significantly reduce error and variability; sensor noise is often the main limiting factor.
- Open-source solutions can match or outperform low-cost commercial devices while offering flexibility and transparency.

### Conclusion

While MATRIX represents a strong low-cost baseline, its performance is constrained by higher sensor noise. Custom open-source IMUs with simple fusion algorithms can outperform MATRIX and approach high-end commercial systems, highlighting the importance of hardware selection in orientation estimation.

### Dissemination


- Alonso, Alejandro & Carro, Gonzalo & Alvarez, Juan & Alvarez, Diego & López, Antonio & Tirado, Diego. (2026). Evaluation of Magnetometer-Less Inertial Sensors for Static and Dynamic Spatial Orientation Estimation. IEEE Transactions on Instrumentation and Measurement. PP. 1-1. 10.1109/TIM.2026.3670515. 

- A. C. Alonso, G. García Carro, J. C. Álvarez, D. Álvarez and A. M. López, "Evaluation of Magnetometer-less Inertial Sensors for Static Spatial Orientation Estimation," 2025 IEEE International Instrumentation and Measurement Technology Conference (I2MTC), Chemnitz, Germany, 2025, pp. 1-6, doi: 10.1109/I2MTC62753.2025.11079213. keywords: {Accelerometers;Measurement units;Accuracy;Inertial sensors;Magnetometers;Noise;Estimation;US Department of Transportation;Standards;Noise level;orientation estimation;inertial unit of measurement;accelerometers;biomechanics},

** JORNADAS DE DOCTORADO Y OTRAS **
