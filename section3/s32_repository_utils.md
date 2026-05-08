---
layout: default
title: "Utils Repository"
nav_order: 2
parent: "Artefactos del sistema"
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
| 1 | sensor_bin_to_csv | bin-file | | | | | |
| 1 | sensor_bin_to_csv | csv-file | | | | | |
| 2 | csv_to_segmented_activity | csv-file | | | | | |
| 2 | csv_to_segmented_activity | excel-activity-log | | | | | |
| 2 | csv_to_segmented_activity | body-segment | | | | | |
| 2 | csv_to_segmented_activity | output | | | | | |
| 2 | csv_to_segmented_activity | sample-init | | | | | |
| 2 | csv_to_segmented_activity | start-time | | | | | |
| 3 | segmented_activity_to_stack | npz-file | | | | | |
| 3 | segmented_activity_to_stack | crop-columns | | | | | |
| 3 | segmented_activity_to_stack | window-size | | | | | |
| 3 | segmented_activity_to_stack | window-overlapping-percent | | | | | |
| 3 | segmented_activity_to_stack | output | | | | | |
| 4 | stack_to_features | stack-file | | | | | |
| 4 | stack_to_features | output | | | | | |
| 5 | aggregate_windows_features | dataset-folder | | | | | |
| 5 | aggregate_windows_features | ml-model | | | | | |
| 5 | aggregate_windows_features | ml-sensors | | | | | |
| 5 | aggregate_windows_features | output-folder | | | | | |
| 6 | model_aggregation | dataset-folder | | | | | |
| 6 | model_aggregation | output-folder | | | | | |
| 6 | model_aggregation | case-id | | | | | |

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