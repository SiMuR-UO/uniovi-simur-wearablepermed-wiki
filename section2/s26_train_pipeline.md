---
layout: default
title: "Train Pipeline"
nav_order: 6
parent: "System Architecture"
---

# Description
This diagram represent the Train pipeline and its steps.

<object
    data="{{ site.baseurl }}/section2/assets/images/train-pipeline.svg"
    type="image/svg+xml"
    style="max-width: 400px; width: 100%;">
</object>

# Steps

- **Step 01**
In this step we train and test our models usin pretrain datasets as inputs and generate model binaries as outputs to be use to predict raw human activity. To remember and resumne, the dataset pretrained in the previous pipeline can be generated using these arguments:

- **Model Type**: Neural Networks models like ESANN or CAPTURE24, models based on characteristics like Random Forest, XGBoot.
- **Segment Body**: Wrist, Thigh or Hip
- **Sensor Channels**: Accelerometer and/or Gyroscope
- **Classes Type**: 4, 8 or 15 classes.
- **Window Size**: a numeric number by default 250.
- **Window Overlapping**: a percent numeric number by default 50%
- **Number of participants**: we can select a subgrout of participants to be train.

In this step we train our models

The default models output created by us are:

- Charatcteritic models like Random Forest using pkl binary format
- Convolution Neural Netoworks like ESANN or CAPTURE24 using h5 binary format

- **Step 02**
In this step we test our models trained to obtain the validation metrics:





