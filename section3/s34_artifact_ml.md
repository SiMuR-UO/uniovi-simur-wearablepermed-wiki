---
layout: default
title: "Machine Learning Artifact"
nav_order: 4
parent: "System Artifacts"
---

# Description
This page documents the Machine Learning training and evaluation code used to produce models from the pretraining datasets created by the utilities pipeline (`section3/s32_artifact_utils.md`).

The repository containing the training code is: https://github.com/SiMuR-UO/uniovi-simur-wearablepermed-ml

This document provides a quick reference for installing, running and understanding the expected dataset formats so the ML code can be integrated with the rest of the pipeline.

## Table of Contents

- [Installation](#installation)
- [Quickstart](#quickstart)
- [Common commands & arguments](#common-commands--arguments)
- [Expected dataset format](#expected-dataset-format)
- [Exporting models / Inference](#exporting-models--inference)
- [Testing](#testing)

## Installation
Recommended Python: 3.12.3.

### Option 1: Install from PyPI

```bash
python -m venv .venv
source .venv/bin/activate
pip install uniovi-simur-wearablepermed-ml
```

### Option 2: Clone and install from source

```bash
git clone https://github.com/SiMuR-UO/uniovi-simur-wearablepermed-ml.git
cd uniovi-simur-wearablepermed-ml
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
# or editable install for development
pip install -e .
```

## Quickstart

Minimal end-to-end steps to train a model using the outputs from the utils artifact:

1. Prepare dataset using the utils pipeline (see `section3/s32_artifact_utils.md`) and create a case-level file such as `data_all.npz` or feature datasets `data_feature_all.npz`.

2. Configure training parameters using a config file (commonly `.vscode/launch.json`). Example config keys: --ml-models, --training-percent, --create-superclasses-CPA-METs.

3. Run training (`trainer.py`). Example:

```bash
python trainer.py --case-id acc_wrist_15_classes_esann \
--case-id-folder /mnt/nvme1n2/git/uniovi-simur-wearablepermed-data/output/Modelos_finales/ \
--ml-models ESANN \
--training-percent 70 \
--validation-percent 20 \
--run-index 1 \
--create-superclasses-CPA-METs
```

### Expected dataset format

The ML code is designed to consume the dataset outputs from the utils pipeline. There is a common NPZ layout used by the overall pipeline:

- Windowed / feature NPZ case files created with `np.savez(...)`:
	- `arr_0` — data (concatenated windows or feature vectors). Array with shape roughly `(n_samples, channels, window_size)` or `(n_samples, n_features)` when features are extracted.
	- `arr_1` — labels aligned to each sample/window.
	- `arr_2` — metadata per sample (subject id).

When running the training process, ensure that the NPZ file containing the windows or features is located in the directory created by concatenating --case-id-folder and --case-id. You should also include the config.cfg file in this folder, in which you have noted the training design details.


4. Testing a trained model (`tester.py`):

```bash
python tester.py --case-id datos_muslo_y_munheca_concatenados \
--case-id-folder /mnt/nvme1n2/git/uniovi-simur-wearablepermed-data/output/RECURADO_DE_DATOS/ \
--model-id ESANN \
--training-percent 70 \
--validation-percent 20 \
--run-index 1 \
--create-superclasses-CPA-METs
```


## Commands & Arguments

| Step | Command | Argument | Type | Optional | Default | Sample | Description |
| ---- | ------- | -------- | ---- | -------- | ------- | ------ | ----------- |
| 1 | `trainer` | `--case-id` | string (name) | No | — | acc_wrist_15_classes_esann | Identifier of the case of study |
| 1 | `trainer` | `case-id-folder` | string (absolute path) | No | — | /mnt/nvme1n2/git/uniovi-simur-wearablepermed-data/output/Modelos_finales/ | Output folder associated with the case of study |
| 1 | `trainer` | `--csv-file` | string (absolute path) | No | — | /path/to/PMP1053_W1_M.csv | Sensor CSV file to be segmented |
| 1 | `trainer` | `--excel-activity-log` | string (absolute path) | No | — | /path/to/PMP1053_RegistroActividades.xlsx | Excel activity log with timetable |
| 1 | `trainer` | `--body-segment` | enum: `PI`,`M`,`C` | No | — | `M` | Body segment code to segment (`PI`=thigh, `M`=wrist, `C`=hip) |
| 1 | `trainer` | `--output` | string (absolute path) | Yes | recommended | /path/to/PMP1053_W1_M_seg.npz | Output .npz file where segments are saved |
| 1 | `trainer` | `--sample-init` | int | Yes | — | `13917630` | Sample index used when sensor close time is missing |
| 2 | `tester` | `--start-time` | time `HH:MM:SS` | Yes | — | `17:37:45` | Start time used to normalize measurements when real times are missing |
| 2 | `tester` | `--npz-file` | string (absolute path) | No | — | /path/to/PMP1053_W1_seg_M.npz | Input .npz file with participant segments |
| 2 | `tester` | `--crop-columns` | format `start:end` or `col1,col2` | No | `1:7` | `1:3` | Number of inertial features to use: accelerometer x,y,z and gyroscope x,y,z (indices 1..7) |
| 2 | `tester` | `--window-size` | int | No | — | `250` | Window size in samples |
| 2 | `tester` | `--window-overlapping-percent` | int (percent) | Yes | `None` | `50` | Overlap percent between windows |
| 2 | `tester` | `--include-not-estructure-data` | flag | Yes | False | 
| 2 | `tester` | `--include-not-estructure-data` | flag | Yes | False |