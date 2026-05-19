---
layout: default
title: "Pipeline Machine Learning Artifact"
nav_order: 5
parent: "System Artifacts"
---

# Description
This page documents the ML pipeline repository that orchestrates model evaluation, testing and case-level experiment runs.

Repository: https://github.com/SiMuR-UO/uniovi-simur-wearablepermed-pipeline-ml

The pipeline artifact is responsible for taking the preprocessed datasets (from `section3/s32_artifact_utils.md`) and the trained model checkpoints (from `section3/s34_artifact_ml.md`) and running evaluation experiments, cross-validation, metric aggregation and producing standardized reports.

## Table of Contents

- [Installation](#installation)
- [Quickstart](#quickstart)
- [Pipeline steps](#pipeline-steps)
- [Commands & arguments](#commands--arguments)
- [Expected dataset format](#expected-dataset-format)
- [Outputs and reports](#outputs-and-reports)
- [Testing](#testing)

## Installation

Recommended Python: 3.10+.

### Option 1: Clone and install from source

```bash
git clone https://github.com/SiMuR-UO/uniovi-simur-wearablepermed-pipeline-ml.git
cd uniovi-simur-wearablepermed-pipeline-ml
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
pip install -e .
```

### Option 2: Docker

If the repository provides a `Dockerfile` or image, run experiments inside the container to avoid environment issues.

## Quickstart

Minimal flow to run an evaluation case:

1. Ensure you have a case-level dataset (e.g. `data_all.npz` or `data_feature_all.npz`) produced by the utils artifact.
2. Have a trained model checkpoint available (e.g. `checkpoint.pt` or similar).
3. Run the pipeline evaluation entrypoint. Example (script names may vary):

```bash
# run a single evaluation
python run_pipeline.py --case-id case_sample --data /path/to/data_all.npz \
	--model /path/to/checkpoint.pt --output /path/to/results

# run cross-validation (k-fold)
python run_pipeline.py --case-id case_sample --data /path/to/data_all.npz \
	--cv-folds 5 --output /path/to/results
```

If the repository uses a CLI entry-point name other than `run_pipeline.py`, replace accordingly. Check the repository README for exact script names and examples.

## Pipeline steps

- Data loading: loads `.npz` dataset and splits into train/val/test or uses cross-validation.
- Model loading: loads the checkpoint and prepares the model for evaluation.
- Evaluation loop: runs predictions, computes metrics (accuracy, precision, recall, F1, confusion matrix), and optionally per-class metrics.
- Aggregation & reporting: collects results across folds/participants and writes human-readable reports and serialized results (CSV/JSON/NPZ).

## Commands & arguments

Common CLI arguments used by the pipeline (names may differ in the repo):

| Argument | Type | Default | Description |
|---|---:|---:|---|
| `--case-id` | string | — | Identifier for the experiment/case |
| `--data` | string | — | Path to input `.npz` dataset (case-level) |
| `--model` | string | — | Path to trained model checkpoint |
| `--output` | string | — | Folder where results, metrics and reports are saved |
| `--cv-folds` | int | 0 | Number of cross-validation folds (0 = use train/test split) |
| `--batch-size` | int | 64 | Batch size for evaluation |
| `--device` | string | cpu | `cpu` or `cuda` device |
| `--metrics` | list | accuracy,f1 | Metrics to compute and report |
| `--seed` | int | 42 | Random seed for reproducibility |

## Expected dataset format

The pipeline expects the datasets produced by the utils and feature extraction steps. Two common NPZ layouts are supported:

- Named keys layout (from `stack_to_features` / `segmented_activity_to_stack`):
	- `WINDOW_CONCATENATED_DATA`: array shape `(n_samples, channels, window_size)` or `(n_samples, n_features)` when features are used.
	- `WINDOW_ALL_LABELS`: labels array aligned with samples.
	- `WINDOW_ALL_METADATA`: metadata per sample (optional).
- Positional arrays layout (case-level aggregated files):
	- `arr_0` — data
	- `arr_1` — labels
	- `arr_2` — metadata

Ensure the `--data` path points to the file type expected by the pipeline configuration.

## Outputs and reports

Typical pipeline outputs:

- Checkpoints (if the pipeline performs training/finetuning during the run).
- Metrics files: CSV/JSON with per-fold and aggregated metrics (accuracy, precision, recall, F1, class-wise metrics).
- Confusion matrices and plots saved as images (PNG/SVG).
- Per-subject or per-class result files for further analysis.

Example output layout:

```
results/case_sample/
	metrics_summary.json
	metrics_per_fold.csv
	confusion_matrix_fold_0.png
	predictions_fold_0.npz
```

## Testing

Run repository tests if available:

```bash
pytest -q
```

## Notes

- This page provides a general, practical reference for the pipeline repository. For exact script names, config schemas and advanced options, consult the repository README at: https://github.com/SiMuR-UO/uniovi-simur-wearablepermed-pipeline-ml
- If you want, puedo clonar el repositorio y extraer los nombres exactos de los scripts y ejemplos reales para reemplazar los comandos genéricos con ejemplos concretos.