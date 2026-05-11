---
layout: default
title: "Utils Repository"
nav_order: 2
parent: "System Artifacts"
---

# Description

This section explain all steps that must be executed sequentially to create the pretrain datasets from raw segment bodies datasets obtained from inertial sensors with accelerometer and gyroscope sensors, located in:

- Wrist
- Thight
- Hip


## Python library arguments

We are going to list all commands and its arguments implemented in this python library:

| Step | Command | Argument | Type    | Optional | Def Value | Sample | Description |
| ---- | ------- | -------- | ------- | -------- | ------ -- | ------ | ----------- |
| 1 | sensor_bin_to_csv | bin-file | string | No | | | |
| 1 | sensor_bin_to_csv | csv-file | string | No | | | |
| 2 | csv_to_segmented_activity | csv-file | string absolute file path | No | | | |
| 2 | csv_to_segmented_activity | excel-activity-log | string absolute file path | No | | | |
| 2 | csv_to_segmented_activity | body-segment | string enum: PI,M,C | No | | | |
| 2 | csv_to_segmented_activity | output | string absolute file path | No | | | |
| 2 | csv_to_segmented_activity | sample-init | number | Yes | | | |
| 2 | csv_to_segmented_activity | start-time | time format: HH:MM:SS | Yes | | | |
| 3 | segmented_activity_to_stack | npz-file | string absolute file path | No | | | |
| 3 | segmented_activity_to_stack | crop-columns | number format: number:number | No | 1:7 | | |
| 3 | segmented_activity_to_stack | window-size | number | No | 250 | | |
| 3 | segmented_activity_to_stack | window-overlapping-percent | string percent | No | 50 | | |
| 3 | segmented_activity_to_stack | output | string absolute folder path | No | | | |
| 4 | stack_to_features | stack-file | string absolute file path | No | | | |
| 4 | stack_to_features | output | string absolute folder path | No | | | |
| 5 | aggregate_windows_features | dataset-folder | string absolute folder path | No | | | |
| 5 | aggregate_windows_features | ml-model | string enum: ESANN,RandomForest| | | | |
| 5 | aggregate_windows_features | ml-sensors | string enum: thigh,wrist,hip | No | | | |
| 5 | aggregate_windows_features | output-folder | string absolute folder path | No | | | |
| 6 | model_aggregation | dataset-folder | string absolute folder path | No | | | |
| 6 | model_aggregation | output-folder | string absolute folder path | No | | | |
| 6 | model_aggregation | case-id | string | No | | | |

## Testing

- Convert binary file to csv:
     ```
     $ sensor_bin_to_csv \
     --bin-file /home/miguel/git/uniovi/simur/uniovi-simur-wearablepermed-utils/data/input/PMP1053/PMP1053_W1_C.BIN \
     --csv-file /home/miguel/git/uniovi/simur/uniovi-simur-wearablepermed-utils/data/input/PMP1053/PMP1053_W1_C.csv
     ```

- Segment all sensor csv files for each participant and correct deviation if exists:
     For participant csv files without some deviation error
     ```
     $ csv_to_segmented_activity \
     --csv-file /home/miguel/git/uniovi/simur/uniovi-simur-wearablepermed-utils/data/input/PMP1053/PMP1053_W1_M.csv \
     --excel-activity-log /home/miguel/git/uniovi/simur/uniovi-simur-wearablepermed-utils/data/input/PMP1053/PMP1053_RegistroActividades.xlsx \
     --body-segment M \
     --output /home/miguel/git/uniovi/simur/uniovi-simur-wearablepermed-utils/data/input/PMP1053/PMP1053_W1_seg_M.npz
     ```

     For participant csv files with some deviation error
     ```
     $ csv_to_segmented_activity \
     --csv-file /home/miguel/git/uniovi/simur/uniovi-simur-wearablepermed-utils/data/input/PMP1053/PMP1053_W1_M.csv \
     --excel-activity-log /home/miguel/git/uniovi/simur/uniovi-simur-wearablepermed-utils/data/input/PMP1053/PMP1053_RegistroActividades.xlsx \
     --body-segment M \
     --output /home/miguel/git/uniovi/simur/uniovi-simur-wearablepermed-utils/data/input/PMP1053/PMP1053_W1_seg_M.npz \
     --sample-init 16088120 \
     --start-time 17:40:00
     ```

- Window segmented files for convolution model likes
     The argument **include-not-estructure-data** can be included if you want add not estructure data
     ```
     $ segmented_activity_to_stack \
     --npz-file /home/miguel/git/uniovi/simur/uniovi-simur-wearablepermed-utils/data/input/PMP1053/PMP1053_W1_seg_M.npz \
     --crop-columns 1:7 \
     --window-size 250 \
     --window-overlapping-percent 50 \
     --output /home/miguel/git/uniovi/simur/uniovi-simur-wearablepermed-utils/data/input/PMP1053/PMP1053_W1_tot_M.npz
     ```

- Extract features from windowed segmented files for random forest model likes
     ```
     $ stack_to_features \
     --stack-file /home/miguel/git/uniovi/simur/uniovi-simur-wearablepermed-utils/data/input/PMP1053/PMP1053_W1_tot_M.npz \
     --output /home/miguel/git/uniovi/simur/uniovi-simur-wearablepermed-utils/data/input/PMP1053/PMP1053_W1_tot_M_features.npz∫
     ```

- Partial aggregation for each participant datasets
     ```
     $ aggregate_windows_features \
     --dataset-folder /home/miguel/git/uniovi/simur/uniovi-simur-wearablepermed-utils/data/input/PMP1003 \
     --ml-models ESANN,RandomForest \
     --ml-sensors thigh,hip,wrist \
     --output-folder /home/miguel/git/uniovi/simur/uniovi-simur-wearablepermed-utils/data/input/PMP1003
     ```

- Total aggregation for all participant datasets to train models
     ```
     $ model_aggregation \
     --dataset-folder /home/miguel/git/uniovi/simur/uniovi-simur-wearablepermed-utils/data/input \
     --output-folder /home/miguel/git/uniovi/simur/uniovi-simur-wearablepermed-utils/data/output \
     --case-id case_sample
     ```