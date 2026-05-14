---
layout: default
title: "Utils Artifact"
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
| 1 | sensor_bin_to_csv | bin-file | string absolute file path | No | | /mnt/nvme0n1p1/wearablepermed/input/PMP1053/PMP1053_W1_M.BIN | Segment Body binary file to convert to csv |
| 1 | sensor_bin_to_csv | csv-file |  string absolute file path | No | | /mnt/nvme0n1p1/wearablepermed/input/PMP1053/PMP1053_W1_M.csv | Segment Body csv file converted from binary one |
| 2 | csv_to_segmented_activity | csv-file | string absolute file path | No | | /mnt/nvme0n1p1/wearablepermed/input/PMP1053/PMP1053_W1_M.csv | Segment Body csv file to be segmented |
| 2 | csv_to_segmented_activity | excel-activity-log | string absolute file path | No | | /mnt/nvme0n1p1/wearablepermed/input/PMP1053/PMP1053_RegistroActividades.xlsx | Excel activity file with all activities timetable |
| 2 | csv_to_segmented_activity | body-segment | string enum: PI,M,C | No | | M | Segment body sensor selected to be segmented |
| 2 | csv_to_segmented_activity | output | string absolute file path | No | | /mnt/nvme0n1p1/wearablepermed/input/PMP1053/PMP1053_W1_M_seg.npz | Stack file name in npz format where segments are saved |
| 2 | csv_to_segmented_activity | sample-init | number | Yes | | 13917630 | When the participant sensor close time was not saved, it represents the unit of measure used to normalize the dataset |
| 2 | csv_to_segmented_activity | start-time | time format: HH:MM:SS | Yes | | 17:37:45 | When the participant sensor close time was not saved, it represents the time of measure used to normalize the dataset|
| 3 | segmented_activity_to_stack | npz-file | string absolute file path | No | | /mnt/nvme0n1p1/wearablepermed/input/PMP1053/PMP1053_W1_M_seg.npz | input of the stack npz file with the participant segment body segments |
| 3 | segmented_activity_to_stack | crop-columns | number format: number:number | No | 1:7 | 1:3 | Number of intertial characteristics to be used: accelerometer x,y,z and gyroscope x,y,z represented by numbers from 1 to 7|
| 3 | segmented_activity_to_stack | window-size | number | No | 250 | 250 | Size of the windows used in the windows step |
| 3 | segmented_activity_to_stack | window-overlapping-percent | string percent | No | 50 | 0 | Overlapping to be used in percentage in the windows steps|
| 3 | segmented_activity_to_stack | output | string absolute folder path | No | | /mnt/nvme0n1p1/wearablepermed/input/PMP1053/PMP1053_W1_tot_M.npz | name of the dataset with the windows |
| 4 | stack_to_features | stack-file | string absolute file path | No | | /mnt/nvme0n1p1/wearablepermed/input/PMP1053/PMP1053_W1_tot_M.npz | |
| 4 | stack_to_features | output | string absolute folder path | No | | /mnt/nvme0n1p1/wearablepermed/input/PMP1053/PMP1053_W1_tot_M_features.npz | |
| 5 | aggregate_windows_features | dataset-folder | string absolute folder path | No | | /mnt/nvme0n1p1/wearablepermed/input/PMP1053 | Where is the participant folfer witl all window and features datasets to be aggregated |
| 5 | aggregate_windows_features | ml-model | string enum: ESANN,RandomForest | No | | RandomForest | for what model must be generate the partial dataset |
| 5 | aggregate_windows_features | ml-sensors | string enum: thigh,wrist,hip | No | | wrist| what segment bodies do you want include in the partial dataset per participant|
| 5 | aggregate_windows_features | output-folder | string absolute folder path | No | | /mnt/nvme0n1p1/wearablepermed/input/PMP1053/PMP1053_W1_all_features.npz.npz | Name of the final partial dataset stack file with all segments bodies to be included|
| 6 | model_aggregation | dataset-folder | string absolute folder path | No | | /mnt/nvme0n1p1/wearablepermed/input | Folder where all participants partical pre-train datasets are located|
| 6 | model_aggregation | output-folder | string absolute folder path | No | | /mnt/nvme0n1p1/wearablepermed/output | Folder where the user cases are located |
| 6 | model_aggregation | case-id | string | No | | case_C_BRF_acc_gyr_15_classes | Name of the user case when the final dataset result will be saved|

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