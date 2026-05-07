---
layout: default
title: "Pipeline Repository"
nav_order: 3
parent: "Artefactos del sistema"
---

# Description

This section explain how we can execute all steps implemented in the previous python module called **utils** in a squencial way, implemented as a pipeline and all arguments that this pipeline offer to execute different user cases. The result of these repository are pretrain datasets to be used to traing models using later repositories.

## Project Arguments

We are going to list the different arguments that can be used for this python module

| Argument | Type    | Optional | Def Value | Sample | Description |
| -------- | ------- | -------- | ------ -- | ------ | ----------- |
| verbose | boolean | Yes | No verbose | | Obtain logs in detail |
| execute-steps | string | No | | "2,3,4,5,6" | number of steps to be executed. the squencial must be in order nad the interval is from 1 to 6 |
| dataset-folder | string | No | | "/mnt/nvme0n1p1/wearablepermed/input" | the folder where all raw participants datasets are located |
| participants-missing-file | string | Yes | All Participants | "/home/simur/git/uniovi-simur-wearablepermed-pipeline/missing_end_datetimes.csv" | file resume for segment participant with missing data, including all sample number at excel time to be normalize. |
| crop-columns | numeric | No | | "1:7" | we can select the characteristics to be include: 1:3 (accelerometer x,y,z); 4:7 (gyroscope x,y,z); 1:7 (all internial characteristics). |
| window-size | numeric | No | | "250" | the number os samples included in thw window |
| window-overlapping-percent | numeric | No | | "50" | overlapping used in the windowed step |
| ml-models | string  | No | | "RandomForest" | dataset type to be generated. Options: RandomForest (used by RandomForest and XGBoots), (CAPTURE24 and ) |
| ml-sensors | string | No | | "thigh,wrist" | segment bodies included in the datasets. Options: thigh, wrist, hip, separated by comma. | 
| output-case-folder | string | No | | "/mnt/nvme0n1p1/wearablepermed/output" | folder where the user case will be created. |
| case-id | string | No | | "case_M_C_BRF_acc_gyr_15_classes" | name os the user case created inside output-case-folder. |
| include-not-estructure-data | boolean | Yes | Only structure data | | If we want include or not the not structure data. |

## Testing

Execute one pipeline from this command:
```
python3 main.py \
    --verbose \
    --execute-steps 2,3,4,5,6 \
    --dataset-folder /home/miguel/git/uniovi/simur/uniovi-simur-wearablepermed-utils/data/input \
    --participants-missing-file /home/miguel/temp/wearablepermed_pipeline/missing_end_datetimes.csv \
    --crop-columns 1:7 \
    --window-size 250 \
    --window-overlapping-percent 50 \
    --ml-models ESANN,RandomForest \
    --ml-sensors thigh,hip,wrist \
    --output-case-folder /home/miguel/git/uniovi/simur/uniovi-simur-wearablepermed-utils/data/output \
    --case-id case_sample
```