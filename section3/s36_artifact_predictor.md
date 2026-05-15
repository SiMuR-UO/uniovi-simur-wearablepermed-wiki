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
wearablepermed-predictor \
	--model-folder /models/case_sample \
	--input-file /data/PMP1053/data_feature_all.npz \
	--output-folder /data/predictions \
	--ml-model RandomForest

# or run the repo entry point
python main.py \
	--model-folder /models/case_sample \
	--input-file /data/PMP1053/data_feature_all.npz \
	--output-folder /data/predictions
```

Batch predict for a folder of participant files:

```bash
wearablepermed-predictor \
	--model-folder /models/case_sample \
	--input-folder /data/input_cases \
	--output-folder /data/predictions \
	--ml-model RandomForest
```

## Commands & Arguments

Below are the typical predictor arguments. Examples follow the conventions used in `s32_artifact_utils.md` and `s33_artifact_pipeline.md`.

| Argument | Type | Optional | Default | Sample | Description |
| -------- | ---- | -------- | ------- | ------ | ----------- |
| `--model-folder` | string | No | — | `/models/case_sample` | Folder containing trained model artifacts and metadata |
| `--model-file` | string | Yes | None | `model.joblib` | Explicit model file inside `--model-folder` to use |
| `--input-file` | string | Yes | — | `/data/PMP1053/data_feature_all.npz` | Single input `.npz` file to predict |
| `--input-folder` | string | Yes | — | `/data/input_cases` | Folder with multiple input files to process in batch |
| `--output-folder` | string | No | — | `/data/predictions` | Output folder where prediction results are written |
| `--ml-model` | string | Yes | — | `RandomForest` | Model identifier or type to select when multiple models are present |
| `--ml-sensors` | string | Yes | — | `thigh,hip,wrist` | Sensors/body segments considered when mapping features to model inputs |
| `--device` | string | Yes | `cpu` | `cpu`/`cuda` | Device to run inference on (for models supporting GPU) |
| `--threshold` | float | Yes | `0.5` | `0.6` | Probability threshold for binary decisions (if applicable) |
| `--verbose` | flag | Yes | False | | Enable verbose logging |

## Input/Output formats

The predictor accepts the same `.npz` artifact formats produced by the pipeline and utils projects.

- Participant-level feature/window files with keys: `WINDOW_CONCATENATED_DATA`, `WINDOW_ALL_LABELS`, `WINDOW_ALL_METADATA` (see `s32_artifact_utils.md`).
- Final case-level files created by `model_aggregation` are positional `.npz` files with `arr_0` (data), `arr_1` (labels), `arr_2` (metadata).

Prediction output:

- Per-input predictions are saved as CSV files in `--output-folder` using one row per sample/window. Typical columns: `sample_index`, `predicted_label`, `predicted_proba` (or one column per class with probabilities).
- A summary JSON may also be written containing model metadata, timestamp, and aggregate metrics (if ground truth is available and evaluation is requested).

Example programmatic usage (quick snippet):

```python
import joblib
import numpy as np

model = joblib.load('/models/case_sample/model.joblib')
data = np.load('/data/PMP1053/data_feature_all.npz')
X = data['arr_0'] if 'arr_0' in data.files else data['WINDOW_CONCATENATED_DATA']
proba = model.predict_proba(X)
pred = model.predict(X)

# Save a simple CSV
import pandas as pd
df = pd.DataFrame({'predicted_label': pred, 'predicted_proba': proba.max(axis=1)})
df.to_csv('/data/predictions/PMP1053_predictions.csv', index=False)
```

## Troubleshooting & common issues

- Model not found: verify `--model-folder` contains the trained model and any required preprocessing metadata (feature scalers, column order, label encoder).
- Input shape mismatch: ensure the input `.npz` matches the features expected by the model (same number/order of features). Use the feature metadata saved at training time to verify.
- GPU errors: confirm model and environment support the requested `--device` and necessary CUDA drivers are installed.
- Permissions: ensure read access to inputs and write access to `--output-folder`.

## Testing

Run unit tests (if present) from the project root:

```bash
pytest -q
```

If the package is installed, you can run a smoke-check by predicting a small sample file and inspecting the CSV output.

---

If you want, I can now create this content in `section3/s36_artifact_predictor.md` using the repository URL you provided and adjust specific command names to match the actual console script in that repo. Indica si lo aplico así.