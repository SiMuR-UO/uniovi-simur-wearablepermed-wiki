---
layout: default
title: "Predictor Artifact"
nav_order: 6
parent: "System Artifacts"
---

# Description

This page documents how to run the predictor component to classify new datasets using trained machine learning models produced by the pipeline.

## Table of Contents

- [Installation](#installation)
- [Quickstart](#quickstart)
- [Commands & Arguments](#commands--arguments)
- [Input/Output formats](#inputoutput-formats)
- [Troubleshooting & common issues](#troubleshooting--common-issues)
- [Testing](#testing)

## Installation

Recommended Python: 3.10+.

### Option 1: Install from PyPI

```bash
python -m venv .venv
source .venv/bin/activate
pip install uniovi-simur-wearablepermed-predictor
```

### Option 2: Install from source (recommended for development)

```bash
git clone https://github.com/SiMuR-UO/uniovi-simur-wearablepermed-predictor.git
cd uniovi-simur-wearablepermed-predictor
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
pip install -e .
```

If the package is installed, run the provided console script. Otherwise run the entry point from the repository root with `python main.py` (or the module entry described in the project README).

### Option 3: Docker

If a Docker image is provided by the project, prefer using it for reproducible inference environments. See the repository README for official images and examples.

Repository: https://github.com/SiMuR-UO/uniovi-simur-wearablepermed-predictor

## Quickstart

Predict a single participant file (features or windows `.npz`) using the installed console script or the module entry point:

```bash
# example using the console script (name may vary if installed)
$ predictor \
--model-type randomforest \
--sensor-channel accelerometer_gyroscope \
--segment-body wrist \
--class-type classes_4 \
--resources-folder /home/miguel/temp/models/wearablepermed_models/input \
--resource-id PMP1024_W1_M.csv \
--cases-folder /home/miguel/temp/models/wearablepermed_models/results \
--case-id case_M_BRF_acc_gyr_4_classes \
--case-file-format csv \
--verbose
```
If you want to use your custom models must execute a command like this:

Batch predict for a folder of participant files:

```bash
predictor \
--model_file /home/miguel/git/uniovi/simur/uniovi-simur-wearablepermed-predictor/models/RandomForest.pkl,
--label_file /home/miguel/git/uniovi/simur/uniovi-simur-wearablepermed-predictor/models/label_encoder.pkl,
--resources-folder /home/miguel/temp/models/wearablepermed_models/input,
--resource-id PMP1024_W1_M.csv,
--cases-folder /home/miguel/temp/models/wearablepermed_models/results,
--case-id case_M_BRF_acc_gyr_4_classes,
--case-file-format csv,
--verbose
```

## Commands & Arguments





## Input/Output formats

TThe predictor accepts .csv files.

Prediction output:

- Per-input predictions are saved as CSV files in `--output-folder` using one row per sample/window. Typical columns: `sample_index`, `predicted_label`, `predicted_proba` (or one column per class with probabilities).
- A summary JSON may also be written containing model metadata, timestamp, and aggregate metrics (if ground truth is available and evaluation is requested).


## Troubleshooting & common issues



## Testing


---

