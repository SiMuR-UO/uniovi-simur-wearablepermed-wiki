---
layout: default
title: "Pretrain Pipeline"
nav_order: 4
parent: "System Architecture"
---

# Description
This diagram represent the Pretrain pipeline and its steps.

<object
    data="{{ site.baseurl }}/section2/assets/images/pretrain-pipeline.svg"
    type="image/svg+xml"
    style="max-width: 1200px; width: 100%;">
</object>

# Steps

- **Step 01**
In this step we convert the raw segment bodies binary files to a semistructure format like csv. The segment bodies could be:
  - Wrist: wrist sensor raw data.
  - Thigh: thigh sensor raw data.
  - Hip: hip sensor raw data.

The result of this step it's a python stack formated in a compress file npz, with a maximun of three files one per each segment body

  - **Step 02**
  In this step we eliminate the potential drift present in each sensor and segment (label) all values using a classification enumerated and activity times writed in the activity excel file during all experiments. The result of this step it's a python stack formated in a compress file npz, with a maximun of three files one per each segment body

  - **Step 03**
  In this step we window the previous segment bodies datasets, using a fix and overlapps windows during all experiments. This datasets we will use to train convolution deep learning models. The final result will be a stack npz format file with all data labeled and windowed. with a maximun of three files one per each segment body

  - **Step 04**
  In this step we extract 91 charateristics from the previous datasets. This dataset it will use to train model that use these charateristics like Randomforest or XGBoots. The final result will be a stack npz format file with all data labeled and windowed with a maximun of three files one per each segment body.

  - **Step 05**
   In this step we concatenated all segments bodies per each participant. We have the capacity to select what segment bodies we can include in this datasets. The final result will be a stack npz format file with all data labeled and windowed or characteristics with a maximun of three files one per each segment body.

  - **Step 06**
  This final step we concatenate all partial aggregations of all participants in only one file to be used in next steps for training model . Again The final result will be a stack npz format file with all data labeled with windowed or characteristics, with only one file concatenating all data.
