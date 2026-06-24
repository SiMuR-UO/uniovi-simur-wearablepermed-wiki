---
layout: default
title: "Pretrain Pipeline"
nav_order: 5
parent: "System Architecture"
---

# Description
This diagram represents the Pretrain pipeline and its steps.

<object
    data="{{ site.baseurl }}/section2/assets/images/pretrain-pipeline.svg"
    type="image/svg+xml"
    style="max-width: 1200px; width: 100%;">
</object>

# Steps

- **Step 01**
  In this step, we convert the raw binary files of the segment bodies into a semi-structured format such as CSV. The segment bodies can be:
  - Wrist: wrist sensor raw data.
  - Thigh: thigh sensor raw data.
  - Hip: hip sensor raw data.

  The result of this step is a Python stack formatted as a compressed npz file, with a maximum of three files, one per segment body.

- **Step 02**
  In this step, we eliminate the potential drift present in each sensor, and segment (label) all values using an enumerated classification and the activity times recorded in the activity Excel file during all experiments. The result of this step is a Python stack formatted as a compressed npz file, with a maximum of three files, one per segment body.

- **Step 03**
  In this step, we apply windowing to the previous segment body datasets, using fixed and overlapping windows across all experiments. These datasets will be used to train convolutional deep learning models. The final result will be a stack in npz format with all data labeled and windowed, with a maximum of three files, one per segment body.

- **Step 04**
  In this step, we extract 91 characteristics from the previous datasets. These datasets will be used to train models that use these characteristics, such as Random Forest or XGBoost. The final result will be a stack in npz format with all data labeled and windowed, with a maximum of three files, one per segment body.

- **Step 05**
  In this step, we concatenate all segment bodies for each participant. We have the capacity to select which segment bodies to include in these datasets. The final result will be a stack in npz format with all data labeled and windowed, or with characteristics, with a maximum of three files, one per segment body.

- **Step 06**
  In this final step, we concatenate all partial aggregations from all participants into a single file, to be used in the next steps for training the model. Again, the final result will be a stack in npz format with all data labeled and windowed, or with characteristics, contained in a single file that concatenates all the data.
