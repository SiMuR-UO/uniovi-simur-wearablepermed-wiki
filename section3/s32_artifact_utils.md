---
layout: default
title: "Utils Artifact"
nav_order: 2
parent: "System Artifacts"
---

# Description

This page documents the utilities used to create pretraining datasets from raw segment-body datasets obtained from inertial sensors (accelerometer and gyroscope) placed on:

- Wrist
- Thigh
- Hip

## Table of Contents

- [Installation](#installation)
- [Quickstart](#quickstart)
- [Commands & Arguments](#commands--arguments)
- [NPZ format (keys)](#npz-format-keys)
- [Body-segment mapping](#body-segment-mapping)
- [Troubleshooting & common issues](#troubleshooting--common-issues)
- [Testing](#testing)

## Installation

## Installation

Recommended Python: 3.10+.

### Option 1: Install from PyPI

```bash
python -m venv .venv
source .venv/bin/activate
pip install uniovi-simur-wearablepermed-utils
```

### Option 2: Install from source

Create and activate a virtual environment and install dependencies:

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
# or install in editable mode while developing:
pip install -e .
```

If you install the package, you can run the console scripts directly. Otherwise, use `python -m`.

### Option 3: Instalación desde Docker Hub




## Quickstart

Minimal end-to-end pipeline:

1. Convert BIN to CSV:

```bash
sensor_bin_to_csv --bin-file /data/PMP1053_W1_C.BIN --csv-file /data/PMP1053_W1_C.csv
```

2. Segment the CSV using the activity log:

```bash
csv_to_segmented_activity --csv-file /data/PMP1053_W1_M.csv \
     --excel-activity-log /data/PMP1053_RegistroActividades.xlsx \
     --body-segment M --output /data/PMP1053_W1_seg_M.npz
```

   - **Optional**: If sensor stop time is missing, use `--sample-init` and `--start-time`:

   ```bash
   csv_to_segmented_activity --csv-file /data/PMP1053_W1_M.csv \
        --excel-activity-log /data/PMP1053_RegistroActividades.xlsx \
        --body-segment M \
        --sample-init 13917630 \
        --start-time 17:37:45 \
        --output /data/PMP1053_W1_seg_M.npz
   ```

   Where:
   - `--sample-init`is the sample to be associated with the timestamp `--start-time`

3. Create windows:

```bash
segmented_activity_to_stack --npz-file /data/PMP1053_W1_seg_M.npz \
     --crop-columns 1:7 --window-size 250 --window-overlapping-percent 50 \
     --output /data/PMP1053_W1_tot_M.npz
```

4. Extract features:

```bash
stack_to_features --stack-file /data/PMP1053_W1_tot_M.npz --output /data/PMP1053_W1_tot_M_features.npz
```

5. Aggregate participant-level datasets with `aggregate_windows_features`.

   - This command takes the per-sensor `.npz` files stored for one participant and merges the selected sensors into a single participant-level file.
   - Output NPZ format: the participant file uses the standard keys `WINDOW_CONCATENATED_DATA`, `WINDOW_ALL_LABELS`, and `WINDOW_ALL_METADATA`.
   - Example:

   ```bash
   aggregate_windows_features \
     --dataset-folder /data/PMP1053 \
     --ml-models ESANN,RandomForest \
     --ml-sensors thigh,hip,wrist \
     --output-folder /data/PMP1053
   ```

6. Combine all participant-level datasets with `model_aggregation`.

   - This command reads all participant partial datasets and creates the final dataset for the selected case.
   - Output NPZ format: `data_all.npz` and `data_feature_all.npz` are saved with positional arrays, so the keys are `arr_0` (data), `arr_1` (labels), and `arr_2` (metadata).
   - Example:

   ```bash
   model_aggregation \
     --dataset-folder /data/input \
     --output-folder /data/output \
     --case-id case_sample
   ```

## Commands & Arguments

| Step | Command | Argument | Type | Optional | Default | Sample | Description |
| ---- | ------- | -------- | ---- | -------- | ------- | ------ | ----------- |
| 1 | `sensor_bin_to_csv` | `--bin-file` | string (absolute path) | No | — | /path/to/PMP1053_W1_M.BIN | Segment Body binary file to convert to CSV |
| 1 | `sensor_bin_to_csv` | `--csv-file` | string (absolute path) | Yes | auto (same name .CSV) | /path/to/PMP1053_W1_M.csv | Output CSV file (if omitted, created next to BIN) |
| 2 | `csv_to_segmented_activity` | `--csv-file` | string (absolute path) | No | — | /path/to/PMP1053_W1_M.csv | Sensor CSV file to be segmented |
| 2 | `csv_to_segmented_activity` | `--excel-activity-log` | string (absolute path) | No | — | /path/to/PMP1053_RegistroActividades.xlsx | Excel activity log with timetable |
| 2 | `csv_to_segmented_activity` | `--body-segment` | enum: `PI`,`M`,`C` | No | — | `M` | Body segment code to segment (`PI`=thigh, `M`=wrist, `C`=hip) |
| 2 | `csv_to_segmented_activity` | `--output` | string (absolute path) | Yes | recommended | /path/to/PMP1053_W1_M_seg.npz | Output .npz file where segments are saved |
| 2 | `csv_to_segmented_activity` | `--sample-init` | int | Yes | — | `13917630` | Sample index used when sensor close time is missing |
| 2 | `csv_to_segmented_activity` | `--start-time` | time `HH:MM:SS` | Yes | — | `17:37:45` | Start time used to normalize measurements when real times are missing |
| 3 | `segmented_activity_to_stack` | `--npz-file` | string (absolute path) | No | — | /path/to/PMP1053_W1_seg_M.npz | Input .npz file with participant segments |
| 3 | `segmented_activity_to_stack` | `--crop-columns` | format `start:end` or `col1,col2` | No | `1:7` | `1:3` | Number of inertial features to use: accelerometer x,y,z and gyroscope x,y,z (indices 1..7) |
| 3 | `segmented_activity_to_stack` | `--window-size` | int | No | — | `250` | Window size in samples |
| 3 | `segmented_activity_to_stack` | `--window-overlapping-percent` | int (percent) | Yes | `None` | `50` | Overlap percent between windows |
| 3 | `segmented_activity_to_stack` | `--include-not-estructure-data` | flag | Yes | False | — | Include non-structured data when present |
| 3 | `segmented_activity_to_stack` | `--output` | string (absolute path) | Yes | — | /path/to/PMP1053_W1_tot_M.npz | Output dataset file with windowed data |
| 4 | `stack_to_features` | `--stack-file` | string (absolute path) | No | — | /path/to/PMP1053_W1_tot_M.npz | Input .npz file with windows |
| 4 | `stack_to_features` | `--output` | string (absolute path) | No | — | /path/to/PMP1053_W1_tot_M_features.npz | Output .npz file with extracted features |
| 5 | `aggregate_windows_features` | `--dataset-folder` | string (absolute path) | No | — | /path/to/PMP1053 | Folder with participant window/feature datasets to aggregate |
| 5 | `aggregate_windows_features` | `--ml-models` | enum list: `ESANN`,`CAPTURE24`,`RandomForest`,`XGBoost` | No | — | `RandomForest` | Target models for generating partial datasets |
| 5 | `aggregate_windows_features` | `--ml-sensors` | enum list: `thigh`,`wrist`,`hip` | No | — | `wrist` | Which body segments to include per participant |
| 5 | `aggregate_windows_features` | `--output-folder` | string (absolute path) | No | — | /path/to/output | Output folder for participant aggregated .npz files |
| 6 | `model_aggregation` | `--dataset-folder` | string (absolute path) | No | — | /path/to/input | Folder containing all participant partial pretrain datasets |
| 6 | `model_aggregation` | `--output-folder` | string (absolute path) | No | — | /path/to/output | Folder where case results are stored |
| 6 | `model_aggregation` | `--case-id` | string | No | — | `case_C_BRF_acc_gyr_15_classes` | Case identifier used to name the output folder |

## NPZ format (keys)

The pipeline uses different `.npz` layouts depending on the step.

### Step 2: `csv_to_segmented_activity`

The output is a compressed `.npz` file created with `np.savez(..., **segmented_activity_data_wpm)`. That means:

- Each key is an activity name.
- Each value is the segmented samples for that activity.
- The stored arrays are the raw segmented blocks before windowing.

Example keys may look like activity labels from the Excel log, for example `CAMINAR`, `SUBIR_ESCALERAS`, `SENTARSE`, etc. The exact keys depend on the activity names in your input log.

### Step 3: `segmented_activity_to_stack`

This step writes a standard dataset `.npz` with these keys:

- `WINDOW_CONCATENATED_DATA`: window tensor with shape approximately `(num_windows, num_channels, window_size)`.
- `WINDOW_ALL_LABELS`: one label per window.
- `WINDOW_ALL_METADATA`: metadata aligned with each window.

The window tensor usually contains the selected inertial channels after cropping, so the data is ready for model training.

### Step 4: `stack_to_features`

This step also writes a standard dataset `.npz` with the same three keys:

- `WINDOW_CONCATENATED_DATA`: feature matrix with one row per window.
- `WINDOW_ALL_LABELS`: one label per feature row.
- `WINDOW_ALL_METADATA`: metadata aligned with each feature row.

### Step 5: `aggregate_windows_features`

This step produces participant-level `.npz` files, again using the same three keys:

- `WINDOW_CONCATENATED_DATA`: concatenated data across the selected sensor types.
- `WINDOW_ALL_LABELS`: labels aligned to the concatenated samples/windows.
- `WINDOW_ALL_METADATA`: metadata aligned to the concatenated samples/windows.

The exact content depends on the selected `--ml-models` and `--ml-sensors` values.

### Step 6: `model_aggregation`

This step writes the final case files `data_all.npz` and, when feature datasets are present, `data_feature_all.npz`.

- These files are created with positional arguments to `np.savez(...)`, so the arrays are stored as `arr_0`, `arr_1`, and `arr_2`.
- `arr_0` is the concatenated data.
- `arr_1` is the labels array.
- `arr_2` is the metadata array.

Example:

```python
import numpy as np
data = np.load('data_all.npz')
print(data.files)  # ['arr_0', 'arr_1', 'arr_2']
```

If you want a quick inspection of any `.npz` file, use:

```python
import numpy as np
data = np.load('some_file.npz')
print(data.files)
for key in data.files:
  print(key, data[key].shape)
```

## Body-segment mapping

- `PI` → thigh
- `M` → wrist
- `C` → hip

Use these codes with `csv_to_segmented_activity`.

## Troubleshooting & common issues

- Timing deviations: use `--sample-init` and/or `--start-time` with `csv_to_segmented_activity` when timestamps are missing or shifted.
- Large files: operations may require significant memory; run per-participant and then aggregate.
- Permissions: ensure read/write permissions for input/output folders.
- Unexpected characters/encoding: verify CSV encoding and remove stray characters.

## Testing

Run tests from the repository root:

```bash
pytest -q
```
