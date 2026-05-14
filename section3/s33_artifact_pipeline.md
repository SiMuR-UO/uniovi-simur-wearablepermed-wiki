---
layout: default
title: "Pipeline Artifact"
nav_order: 3
parent: "System Artifacts"
---

# Description

This section explains how to run the end-to-end pipeline that orchestrates the steps provided by the `utils` repository to produce pretrain datasets. The pipeline executes the same processing stages documented in `s32_artifact_utils.md` (segmentation, windowing, feature extraction, participant aggregation and model case creation) in a sequential manner and exposes flags to control each step.

## Table of Contents

- [Installation](#installation)
- [Quickstart](#quickstart)
- [Commands & Arguments](#commands--arguments)
- [NPZ format (keys)](#npz-format-keys)
- [Body-segment mapping](#body-segment-mapping)
- [Troubleshooting & common issues](#troubleshooting--common-issues)
- [Testing](#testing)

## Installation

Recommended Python: 3.10+.

### Option 1: Install from PyPI

```bash
python -m venv .venv
source .venv/bin/activate
pip install uniovi-simur-wearablepermed-pipeline
```

### Option 2: Install from source

```bash
git clone https://github.com/SiMuR-UO/uniovi-simur-wearablepermed-pipeline.git
cd uniovi-simur-wearablepermed-pipeline
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
pip install -e .
```

If you installed the package, run the provided console script; otherwise run the entry point from the repo root with `python main.py`.

## Quickstart

Minimal end-to-end pipeline (pipeline runs steps 2..6 implemented by the `utils` tools):

Run the pipeline (example executing segmentation, windowing, features, aggregation and model aggregation):

```bash
pipeline \
    --verbose \
    --execute-steps 1,2,3,4,5,6 \
    --dataset-folder /data/input \
    --participants-missing-file /data/missing_end_datetimes.csv \
    --crop-columns 1:7 \
    --window-size 250 \
    --window-overlapping-percent 50 \
    --ml-models ESANN,RandomForest \
    --ml-sensors thigh,hip,wrist \
    --output-case-folder /data/output \
    --case-id case_sample

```

Notes:
- The pipeline will internally call the same utilities described in `s32_artifact_utils.md` (for example `csv_to_segmented_activity`, `segmented_activity_to_stack`, `stack_to_features`, `aggregate_windows_features`, and `model_aggregation`).
- Use `--sample-init` and `--start-time` when sensor timestamps are missing or need normalization; their meaning matches the options of `csv_to_segmented_activity`.

## Commands & Arguments

Below are the main pipeline arguments; examples and names follow the conventions used in `s32_artifact_utils.md`.

| Argument | Type | Optional | Default | Sample | Description |
| -------- | ---- | -------- | ------- | ------ | ----------- |
| `--verbose` | flag | Yes | False | | Enable verbose logging |
| `--execute-steps` | string | No | — | `2,3,4,5,6` | Comma-separated list of steps to execute (1..6) in ascending order |
| `--dataset-folder` | string | No | — | `/data/PMP1053` | Folder with raw participant files (CSV/BIN and logs) |
| `--participants-missing-file` | string | Yes | — | `/data/missing_end_datetimes.csv` | CSV with participant/sample mappings to fix missing timestamps |
| `--crop-columns` | string | No | `1:7` | `1:7` | Columns/features to keep (e.g. `1:3` accel, `4:7` gyro) |
| `--window-size` | int | No | — | `250` | Number of samples per window |
| `--window-overlapping-percent` | int | Yes | 0 | `50` | Window overlap percentage |
| `--ml-models` | string | No | — | `ESANN,RandomForest` | Target dataset types/models to generate |
| `--ml-sensors` | string | No | — | `thigh,hip,wrist` | Body segments to include (comma-separated) |
| `--output-case-folder` | string | No | — | `/data/output` | Output folder where the case will be written |
| `--case-id` | string | No | — | `case_sample` | Case identifier (folder name inside output-case-folder) |
| `--include-not-estructure-data` | flag | Yes | False | | Include non-structured data when present |
| `--sample-init` | int | Yes | — | `13917630` | Sample index to associate with `--start-time` when sensor timestamps are missing |
| `--start-time` | `HH:MM:SS` | Yes | — | `17:37:45` | Start time to normalize measurements when real times are missing |

## NPZ format (keys)

The pipeline produces the same `.npz` layouts described in `s32_artifact_utils.md` for each step:

- Step 2 (`csv_to_segmented_activity`) outputs a compressed `.npz` where each key is an activity name and the value is the array of segmented samples for that activity.
- Step 3 (`segmented_activity_to_stack`) writes `.npz` with keys: `WINDOW_CONCATENATED_DATA`, `WINDOW_ALL_LABELS`, `WINDOW_ALL_METADATA`.
- Step 4 (`stack_to_features`) writes a features `.npz` with the same three keys (rows = windows/features).
- Step 5 (`aggregate_windows_features`) produces participant-level `.npz` files with the same three keys, concatenating selected sensors.
- Step 6 (`model_aggregation`) writes final case files `data_all.npz` and optionally `data_feature_all.npz` stored as positional arrays (`arr_0`, `arr_1`, `arr_2`).

Example inspection:

```python
import numpy as np
data = np.load('data_all.npz')
print(data.files)  # ['arr_0', 'arr_1', 'arr_2']
```

## Body-segment mapping

- `PI` → thigh
- `M` → wrist
- `C` → hip

Use these codes consistently with the `csv_to_segmented_activity` utility and pipeline flags.

## Troubleshooting & common issues

- Timing deviations: use `--sample-init` and/or `--start-time` with the pipeline (these are forwarded to `csv_to_segmented_activity`).
- Large files: run per-participant and then aggregate to avoid memory spikes.
- Permissions: ensure read/write access for input and output folders.
- Unexpected characters/encoding: verify CSV encoding and clean stray characters.

## Testing

Run tests from the pipeline repository root:

```bash
pytest -q
```

Repository: https://github.com/SiMuR-UO/uniovi-simur-wearablepermed-pipeline — see the project root for the entry point and concrete flag definitions (if installed, run the provided console script `wearablepermed-pipeline`).