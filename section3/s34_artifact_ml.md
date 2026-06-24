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
- [Commands & arguments](#common-commands--arguments)

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

The `tester.py` script generates confusion matrices in PNG format for each iteration, along with the accuracy and F1-score metrics obtained using the respective model.


## Commands & Arguments

| Step | Command | Argument | Type | Optional | Default | Sample | Description |
| ---- | ------- | -------- | ---- | -------- | ------- | ------ | ----------- |
| 1 | `trainer` | `--case-id` | string | No | — | acc_wrist_15_classes_esann | Identifier of the case of study |
| 1 | `trainer` | `--case-id-folder` | string (absolute path) | No | — | /mnt/nvme1n2/git/uniovi-simur-wearablepermed-data/output/Modelos_finales/ | Output folder associated with the case of study |
| 1 | `trainer` | `--ml-models` | enum: `RandomForest`,`XGBoost`,`ESANN`, `CAPTURE24` | No | — | RandomForest | Model which we want to train |
| 1 | `trainer` | `--training-percent` | int (percent) | No | 70 | 70 | Percentage used to split the dataset into the train subset |
| 1 | `trainer` | `--validation-percent` | int (percent) | No | 0 | 20 | Percentage used to split the dataset into the validation subset |
| 1 | `trainer` | `--run-index` | int | No | — | 1 | Index that indicates the execution number for each repetition of the experiment (train step) |
| 1 | `trainer` | `--create-superclasses-CPA-METs` | string | Yes | — | `--create-superclasses-CPA-METs` | Flag that indicates if we want to train a model with a superclasses dataset |
| 2 | `tester` | `--case-id` | string | No | — | datos_muslo_y_munheca_concatenados | Identifier of the case of study |
| 2 | `tester` | `--case-id-folder` | string (absolute path) | No | — | /mnt/nvme1n2/git/uniovi-simur-wearablepermed-data/output/RECURADO_DE_DATOS/ | Output folder associated with the case of study |
| 2 | `tester` | `--model-id` | enum: `RandomForest`,`XGBoost`,`ESANN`, `CAPTURE24` | No | — | RandomForest | Model which we want to test |
| 2 | `tester` | `--training-percent` | int (percent) | No | 70 | 70 | Percentage used to split the dataset into the train subset |
| 2 | `tester` | `--validation-percent` | int (percent) | Yes | 0 | 20 | Percentage used to split the dataset into the validation subset |
| 2 | `tester` | `--run-index` | int | No | — | 1 | Index that indicates the execution number for each repetition of the experiment (test step) | 
| 2 | `tester` | `--create-superclasses-CPA-METs` | string | Yes | — | `--create-superclasses-CPA-METs` | Flag that indicates if we want to test a model with a superclasses dataset |