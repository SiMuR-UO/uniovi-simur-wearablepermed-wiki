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
- [Training workflow](#training-workflow)
- [Common commands & arguments](#common-commands--arguments)
- [Expected dataset format](#expected-dataset-format)
- [Exporting models / Inference](#exporting-models--inference)
- [Testing](#testing)

## Installation
Recommended Python: 3.10+.

### Option 1: Clone and install from source

```bash
git clone https://github.com/SiMuR-UO/uniovi-simur-wearablepermed-ml.git
cd uniovi-simur-wearablepermed-ml
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
# or editable install for development
pip install -e .
```

### Option 2: Docker (if provided by repo)

If the repository contains a `Dockerfile` or `docker` setup, prefer running training inside the container to avoid host dependency issues. Check the repo README for available images or example `docker build` / `docker run` commands.

## Quickstart

Minimal end-to-end steps to train a model using the outputs from the utils artifact:

1. Prepare dataset using the utils pipeline (see `section3/s32_artifact_utils.md`) and create a case-level file such as `data_all.npz` or feature datasets `data_feature_all.npz`.

2. Configure training parameters using a config file (commonly `configs/*.yaml` or a CLI argument). Example config keys: model type, optimizer settings, scheduler, data paths.

3. Run training (repository may expose a `train.py` or an entry-point). Example:

```bash
python train.py --config configs/case_sample.yaml --data /path/to/data_all.npz --output /path/to/experiments/case_sample
```

4. Evaluate a trained model:

```bash
python evaluate.py --checkpoint /path/to/experiments/case_sample/checkpoint.pt --data /path/to/data_all.npz
```

5. Export model for inference (ONNX / TorchScript) if supported by the repo:

```bash
python export_model.py --checkpoint /path/to/checkpoint.pt --output /path/to/export/model.onnx
```

## Training workflow

Typical workflow implemented by the repo (verify exact script names in the repository README):

- Data loader reads `.npz` datasets produced by the utils artifact and converts them to PyTorch/TensorFlow datasets.
- Model definition (configurable) is instantiated and moved to device (`cpu`/`cuda`).
- Training loop handles logging, checkpointing and optional early stopping.
- Evaluation script computes metrics on the test split and may produce confusion matrices and report files.

## Common commands & arguments

The repository usually accepts a common set of CLI arguments or config keys. The exact names may vary; use these as a quick reference.

| Argument | Type | Default | Description |
|---|---:|---:|---|
| `--config` | string | — | Path to YAML/JSON config with hyperparameters |
| `--data` | string | — | Path to input `.npz` dataset (case-level) |
| `--output` | string | — | Folder where experiment checkpoints and logs are saved |
| `--epochs` | int | 100 | Number of training epochs |
| `--batch-size` | int | 64 | Batch size for training |
| `--learning-rate` | float | 1e-3 | Initial optimizer learning rate |
| `--device` | string | cpu | `cpu` or `cuda` device to run on |
| `--seed` | int | 42 | Random seed for reproducibility |

## Expected dataset format

The ML code is designed to consume the dataset outputs from the utils pipeline. There are two common NPZ layouts used by the overall pipeline:

- Windowed / feature NPZ with named keys (as produced by the utils commands):
	- `WINDOW_CONCATENATED_DATA` — array with shape roughly `(n_samples, channels, window_size)` or `(n_samples, n_features)` when features are extracted.
	- `WINDOW_ALL_LABELS` — labels aligned to each sample/window.
	- `WINDOW_ALL_METADATA` — optional metadata per sample.

- Aggregated case files created with `np.savez(...)` positional arrays (final case files):
	- `arr_0` — data (concatenated windows or feature vectors)
	- `arr_1` — labels
	- `arr_2` — metadata

When running training, ensure the `--data` argument points to the correct file type expected by the configuration (raw windows vs. feature matrix).

## Exporting models / Inference

After training, the repo may provide utilities to export checkpoints to inference formats (TorchScript, ONNX) or a small `predict.py` for single-subject inference. Example usage:

```bash
python predict.py --model /path/to/export/model.onnx --input /path/to/participant.npz
```

## Testing

Run tests from the repository root if available:

```bash
pytest -q
```

## Notes

- This page summarizes the typical structure and commands. For exact script names, config keys and advanced options, check the repository README at: https://github.com/SiMuR-UO/uniovi-simur-wearablepermed-ml
- The ML code depends on the dataset layout produced by the utils artifact (`section3/s32_artifact_utils.md`). Keep both repositories' versions in sync when upgrading processing pipelines.