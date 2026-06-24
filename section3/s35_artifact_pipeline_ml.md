---
layout: default
title: "Pipeline Machine Learning Artifact"
nav_order: 5
parent: "System Artifacts"
---

# Description
This page documents the ML pipeline repository that orchestrates model evaluation, testing and case-level experiment runs.

Repository: https://github.com/SiMuR-UO/uniovi-simur-wearablepermed-pipeline-ml

The pipeline artifact is responsible for taking the preprocessed datasets (from `section3/s32_artifact_utils.md`) and the trained model checkpoints (from `section3/s34_artifact_ml.md`) and running evaluation experiments.


## Table of Contents

- [Installation](#installation)
- [Quickstart](#quickstart)
- [Commands & arguments](#commands--arguments)

## Installation

Recommended Python: 3.12.3.

### Option 1: Install from PyPI

```bash
python -m venv .venv
source .venv/bin/activate
pip install uniovi-simur-wearablepermed-pipeline-ml
```

### Option 2: Clone and install from source

```bash
git clone https://github.com/SiMuR-UO/uniovi-simur-wearablepermed-pipeline-ml.git
cd uniovi-simur-wearablepermed-pipeline-ml
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
pip install -e .
```


## Quickstart

Minimal flow to run an evaluation case:

1. Ensure you have a case-level dataset (e.g. `data_all.npz` or `data_feature_all.npz`) produced by the utils artifact.
2. Run the pipeline:

```bash
# run a single evaluation
python pipeline.py --dataset-case-root /mnt/nvme1n2/git/uniovi-simur-wearablepermed-data/output/RECURADO_DE_DATOS
```

## Pipeline steps

- Data loading: loads `.npz` dataset and splits into train/val/test.
- Training and Evaluation loop for each model: runs trainings and predictions, computes metrics (accuracy, F1-score, confusion matrix), and per-class metrics.
- Aggregation and reporting: collects results across participants and writes human-readable reports and serialized results (NPZ, txt).

## Commands & arguments

| Step | Command | Argument | Type | Optional | Default | Sample | Description |
| ---- | ------- | -------- | ---- | -------- | ------- | ------ | ----------- |
| 1 | `pipeline` | `--dataset-case-root` | string (path) | No | — | /mnt/nvme1n2/git/uniovi-simur-wearablepermed-data/output/RECURADO_DE_DATOS | Folder in which we have located all cases of study we want to execute and analyze |