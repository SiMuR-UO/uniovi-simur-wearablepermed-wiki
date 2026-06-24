---
layout: default
title: "Train Pipeline"
nav_order: 6
parent: "System Architecture"
---

# Description
This diagram represents the Train pipeline and its steps.

<object
    data="{{ site.baseurl }}/section2/assets/images/train-pipeline.svg"
    type="image/svg+xml"
    style="max-width: 550px; width: 100%;">
</object>

# Steps
- **Step 01**
In this step we train and test our models using pretrain datasets as inputs and generate model binaries as outputs to be used to predict raw human activity. As a reminder, the dataset pretrained in the previous pipeline can be generated using these arguments:

- **Model Type**: Neural Network models like ESANN or CAPTURE24, models based on characteristics like Random Forest or XGBoost.
- **Segment Body**: Wrist, Thigh or Hip.
- **Sensor Channels**: Accelerometer and/or Gyroscope
- **Classes Type**: 4, 8 or 15 classes.
- **Window Size**: a numeric value, 250 by default.
- **Window Overlapping**: a percentage value, 50% by default.
- **Number of participants**: we can select a subgroup of participants to be trained.

In this step we train our models.

The default model outputs created by us are:

- Characteristic models like Random Forest, using pkl binary format.
- Convolutional Neural Networks like ESANN or CAPTURE24, using h5 binary format.

- **Step 02**
In this step we test our trained models to obtain the validation metrics:
