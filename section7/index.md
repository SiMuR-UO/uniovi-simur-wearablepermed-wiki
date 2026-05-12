---
layout: default
title: "Technical Reports"
nav_order: 9
has_children: true
---

# Technical Reports

This section compiles technical reports and research studies conducted by SiMuR related to the WearablePerMed project, including evaluations of wearable sensors, algorithm development, and human activity recognition studies using multimodal sensor data.

The technical reports can be accesed in the corresponding [Git repository](https://github.com/SiMuR-UO/uniovi-simur-wearablepermed-technical-reports).

## IT-001. Evaluación Sensor Matrix.

This technical report presents the analysis, characterization, and calibration of the MATRIX inertial measurement unit (IMU) developed within the WearablePerMed project. The device integrates a triaxial accelerometer and gyroscope, enabling the measurement of both linear acceleration and angular velocity of human motion.

The study evaluates the main metrological properties of the sensor, including noise levels at rest, axis orthogonality, and measurement accuracy. To improve signal quality, experimental calibration procedures are applied to both the accelerometers and gyroscopes, significantly reducing offset errors and scale factors.

Additionally, the capability of the sensor to estimate spatial orientation and compute joint angles is analyzed using quaternion-based sensor fusion techniques. The results show that the system provides reliable and stable estimates, with low errors under controlled conditions.

Overall, this work validates the MATRIX IMU as a suitable and robust solution for human motion analysis applications, both in controlled environments and in real-life monitoring scenarios.

## IT-002. Descripción Señales Biomecánicas 

This technical report presents the characterization of biomechanical signals recorded at the thigh using inertial sensors during various representative human locomotion activities, within the framework of the WearablePerMed project. The main objective is to identify characteristic patterns in acceleration and angular velocity signals associated with movements such as sit-to-stand transitions, stair ascent and descent, walking, cycling, and running.

To achieve this, an approach based on the analysis of relevant components of inertial signals is employed, mainly considering the sagittal, vertical, and medio-lateral axes, together with a preprocessing stage based on filtering tailored to each type of activity. The study describes the distinctive patterns of each movement, enabling the identification of key events such as transition phases, gait cycles, and impact moments.

The results show that each activity exhibits distinct dynamic signatures: sit-to-stand transitions are characterized by abrupt changes between steady states, stair activities by synchronized peaks in acceleration and angular velocity, walking by cyclic patterns associated with gait events, cycling by periodic sinusoidal signals, and running by higher-amplitude, higher-intensity oscillations.

Overall, this work provides a solid foundation for the understanding and interpretation of thigh-mounted inertial signals, supporting the development of activity recognition algorithms and human movement analysis in both controlled and real-world environments.

## IT-003. Identificacion Patrones Señales Biomecanicas

This technical report presents a study focused on the identification and characterization of biomechanical signal patterns in the thigh and wrist using public datasets, within the context of the WearablePerMed project. The main objective is to define representative patterns of acceleration and angular velocity in key human mobility activities such as walking, running, cycling, and stair ascent/descent.

The methodology is based on the selection of reference datasets, the automatic segmentation of movement cycles (strides or pedaling cycles) using medio-lateral gyroscope signals, and the subsequent construction of average patterns through temporal alignment and normalization of the signals. The representativeness of the patterns is evaluated using statistical techniques based on correlation coefficients, allowing their stability and generalization capability to be quantified.

The results show that the most consistent patterns are found at the thigh and are mainly associated with the dominant axes of motion, particularly the antero-posterior and vertical accelerations, and the medio-lateral angular velocity. In contrast, wrist signals exhibit higher variability and lower stability, except for certain axes associated with global movement. Additionally, cyclic activities such as walking, running, and cycling produce more robust patterns than more complex or irregular movements such as descending stairs.
Overall, this work demonstrates the feasibility of using public datasets to define representative biomechanical patterns, providing a scalable and data-driven approach to human motion analysis and the development of physical activity recognition systems.

## IT-004. Clasificación con Neuronal

This technical report presents a study on physical activity classification using inertial signals and deep neural networks, within the framework of the WearablePerMed project. The main objective is to evaluate the capability of acceleration and angular velocity data recorded at the wrist and thigh to identify different human activities, as well as to analyze the impact of sensor location on classification performance.

The methodology is based on the use of public datasets containing recordings of multiple activities, from which preprocessing steps are applied, including axis alignment, resampling, and segmentation into temporal windows. On this data, a convolutional neural network is implemented to process multivariate signals, evaluating three configurations: combined use of wrist and thigh sensors, exclusive use of the thigh, and exclusive use of the wrist.

The results show that the combination of sensors at both locations provides the best performance, achieving high classification accuracy. It is also observed that wrist-based data can outperform thigh-based data in activities where upper-body motion is more relevant, whereas the thigh proves to be more informative for locomotion-related activities.
Overall, this work demonstrates the effectiveness of neural networks for physical activity recognition from inertial signals, highlighting the importance of sensor placement and showing that multi-sensor fusion significantly improves classification performance.

## IT-005. Exploración de Datasets PMP1020_1024_1047_1049_1051

This technical report presents an exploratory analysis of several datasets from the WearablePerMed project (PMP1020, PMP1024, PMP1047, PMP1049, and PMP1051), focusing on the validation, segmentation, and correction of data acquired from MATRIX inertial sensors. The main objective is to assess data quality, identify inconsistencies, and establish robust procedures for processing data in both structured activities and free-living conditions.

The study begins with the experimental verification of the IMU sampling frequency, confirming values close to the theoretical 25 Hz. Next, activity segmentation is analyzed using both manual logs and inertial signals, assessing their consistency and identifying temporal misalignments and inconsistencies in some datasets. Cross-validation is also performed using external data sources, such as force platform measurements and wearable camera images, to ensure correct interpretation of the signals.

In the case of free-living activity, the feasibility of segmenting events based on visual information and timestamps is demonstrated, showing that accelerometer and gyroscope signals consistently reflect different activity contexts. Sleep periods are also identified by combining activity diaries and inertial data.

One of the key contributions of this work is the detection and correction of errors in manual annotations, as well as the compensation of sensor time drift using timestamp adjustment techniques. This process enables proper alignment of IMU data with external temporal references, improving overall data quality.

Overall, this work provides a solid methodological foundation for the preprocessing, validation, and exploitation of accelerometry datasets in physical activity studies, which is essential for ensuring the reliability of subsequent analysis and classification models in real-world environments.

## IT-006. Evaluacion Dataset

This technical report presents a preliminary analysis of the WearablePerMed dataset with the aim of assessing its integrity, identifying potential inconsistencies, and establishing correction procedures that enable its use in subsequent physical activity studies. The work focuses on the validation of accelerometry data and associated records, including activity logs and additional measurements.

The analysis process includes verification of data files, correct conversion and temporal segmentation of the signals, and review of the consistency between inertial data and activity annotations. During this process, various issues are identified, mainly related to file format errors, inconsistencies in dates and times, missing data, duplicates, and failures in segmentation procedures.

To address these issues, different correction strategies are applied, including manual adjustments, reconstruction of timestamps, removal of inconsistent data, and normalization of files. Particular attention is given to the correction of sensor time drift, a critical aspect for proper data synchronization.
As a result of the analysis, dataset participants are classified according to the quality and usability of their data, distinguishing between correct data, corrected data, partially usable data, and unusable data. This classification provides a solid basis for sample selection in future studies.

Overall, this work provides a systematic evaluation of the quality of the WearablePerMed dataset, defining key procedures for its cleaning and validation, which is essential to ensure the reliability of subsequent analyses in physical activity recognition and health-related variable estimation.

## IT-007 Step Definition

This document defines an operational protocol for step counting based on observable biomechanical criteria, aimed at ensuring consistency and reproducibility in the identification of gait events. A step is defined as an event in which the foot is displaced, supports load, and produces a visible forward progression of the trunk, with the counting moment corresponding to the initial contact of the foot with the ground.

The protocol distinguishes between different types of events, including valid steps within the gait pattern, closing steps that do not generate forward body progression, and non-purposeful steps that involve foot movement without significant progression. It also introduces specific criteria for counting during turning movements, considering only those foot contacts with load that contribute to effective body reorientation, while excluding pivoting movements without displacement.

The proposal is supported by existing literature to formalize an operational definition of a step based on three key elements: foot movement, body weight transfer, and inclusion within a repetitive gait pattern. Overall, this protocol provides a systematic framework for manual annotation and for the validation of step-counting algorithms in real-world physical activity studies.

## IT-008 Informe_Pasos

This technical report evaluates the performance of step-counting algorithms based on inertial measurement units (IMUs) placed at different body locations: hip, thigh, and wrist. The main objective is to analyze the accuracy of the developed methods across different walking scenarios, including controlled conditions and more complex situations with motion interference, such as hand use or changes in direction.

The methodology combines controlled experiments with validation on real data. In the initial study, a user performs walking trials while wearing IMUs and commercial reference devices, comparing the obtained results with manual step counts. Subsequently, the algorithms are evaluated using data from real participants of the WearablePerMed project under different movement conditions.

The results show that the algorithms provide high accuracy when the sensor is placed on the hip or thigh, with errors generally below 2%. For the wrist, accuracy largely depends on arm behavior, with increased error observed when natural movement is altered. In further analyses using real-world data, errors remain moderate overall, although they may increase under specific conditions or depending on the individual.
Overall, the study confirms the feasibility of step counting using IMUs in different body locations, highlighting that the hip and thigh offer greater robustness, while the wrist is more sensitive to external perturbations. These findings provide a solid basis for the development and validation of step-counting algorithms in real-life conditions.

## IT-009 Algoritmos Cuenta de Pasos - dataset WPM

This technical report presents the development and evaluation of step-counting algorithms applied to the WearablePerMed (WPM) dataset, with the aim of analyzing their performance across different daily-life activities. Both structured and semi-structured activities are considered, including various locomotion patterns as well as standing tasks, all annotated with reference values obtained through expert step counting.

The proposed methodology includes the design of multiple algorithms based on accelerometry signals collected from different body locations (hip, thigh, and wrist). These methods are grouped according to their technical approach, including gait event detection, threshold-based analysis, and direct step count estimation. Performance is evaluated using metrics such as the mean absolute percentage error (MAPE), complemented by statistical analyses and graphical representations of error distributions.

The results show that the developed algorithms are capable of estimating the number of steps with generally low error, with median MAPE values around 5% across various activities. Consistent performance is observed in locomotion tasks such as walking, jogging, and walking with load, although outliers are identified, mainly associated with annotation errors or specific subject conditions.

The study also highlights the difficulty of defining and counting steps in non-strictly locomotor activities, where the relationship between inertial signals and the actual displacement of the center of gravity is not straightforward. In this context, a step definition based on the anteroposterior displacement of the body is proposed, which introduces limitations in certain static tasks involving limb movement.
Overall, this work demonstrates the feasibility of step-counting algorithms in real-world conditions and emphasizes the importance of an operational definition of a step, the quality of the ground truth, and inter-subject variability for properly interpreting results in uncontrolled environments.