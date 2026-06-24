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
- [Model Types & Configuration](#model-types--configuration)
- [Docker Deployment](#docker-deployment)
- [Input/Output formats](#inputoutput-formats)
- [Troubleshooting & common issues](#troubleshooting--common-issues)
- [Testing](#testing)
- [Project Structure](#project-structure)
- [Contributing](#contributing)

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
If you want to use your custom models, you must execute a command like this:

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

### Using the Console Script

```bash
predictor \
  --model-type randomforest \
  --sensor-channel accelerometer_gyroscope \
  --segment-body wrist \
  --class-type classes_4 \
  --resources-folder /path/to/input \
  --resource-id PMP1024_W1_M.csv \
  --cases-folder /path/to/output \
  --case-id case_M_BRF_acc_gyr_4_classes \
  --case-file-format csv \
  --verbose
```

### Required Arguments (marked with *)

| Argument | Description | Example |
|----------|-------------|---------|
| `--segment-body *` | Body location: thigh, wrist, hip | `wrist` |
| `--resources-folder *` | Root directory for input resources | `/home/user/data/input` |
| `--resource-id *` | CSV file identifier to process | `PMP1024_W1_M.csv` |

### Model Configuration Arguments

| Argument | Description | Default | Values |
|----------|-------------|---------|--------|
| `--model-type` | Model algorithm | - | esann, capture24, randomforest, xgboost |
| `--sensor-channel` | Sensor types used | - | accelerometer, accelerometer_gyroscope |
| `--class-type` | Classification targets | - | classes_4, classes_15 |

### Optional Arguments

| Argument | Description | Default |
|----------|-------------|---------|
| `--model-file` | Path to custom model file (.pkl) | - |
| `--label-file` | Path to label encoder file (.pkl) | - |
| `--cases-folder` | Output directory for results | current directory |
| `--case-id` | Unique case identifier | - |
| `--case-file-format` | Output format | npz |
| `--is-label-export` | Export predictions as labels | False |
| `--is-database-export` | Save predictions to database | False |
| `--verbose` | Enable verbose logging | False |





## Model Types & Configuration

### Default Model Configuration

All pre-trained models offered by the predictor are configured with:

- **Window size**: 250 samples
- **Overlapping**: 50%
- **Model scope**: Individual segment-body models (Wrist, Thigh, or Hip)

### Currently Implemented Models

| Model Type | Description | Status |
|-----------|-------------|--------|
| RandomForest | Tree-based ensemble learning | Fully implemented |
| XGBoost | Gradient boosting framework | Available |
| ESANN | Neural network approach | Available |
| Capture24 | Activity classification | Available |

### Pre-trained Models on Hugging Face

The predictor can download and use pre-trained models from Hugging Face. These models are pre-trained on the WearablePerMed dataset with specific sensor channels and body segments.

## Docker Deployment

### Building the Docker Image

```bash
docker build -t wearablepermed-predictor:1.18.0 .
```

### Running with Docker

```bash
docker run \
  --rm \
  -u $(id -u):$(id -g) \
  -v /path/to/input:/data/input \
  -v /path/to/output:/data/output \
  simuruo/wearablepermed-predictor:1.18.0 \
  --model-type randomforest \
  --sensor-channel accelerometer_gyroscope \
  --segment-body wrist \
  --class-type classes_4 \
  --resources-folder /data/input \
  --resource-id PMP1024_W1_M.csv \
  --cases-folder /data/output \
  --case-id case_M_BRF_acc_gyr_4_classes \
  --verbose
```

### Interactive Shell in Container

```bash
docker run \
  --rm \
  -it \
  -u $(id -u):$(id -g) \
  -v /path/to/input:/data/input \
  -v /path/to/output:/data/output \
  --entrypoint sh \
  simuruo/wearablepermed-predictor:1.18.0
```

### Publishing to Docker Hub

```bash
# Tag the image
docker tag wearablepermed-predictor:1.18.0 simuruo/wearablepermed-predictor:1.18.0

# Login (requires credentials)
docker login -u simuruo

# Push to registry
docker push simuruo/wearablepermed-predictor:1.18.0
```

## Input/Output formats

The predictor accepts .csv files.

Prediction output:

- Per-input predictions are saved as CSV files in `--output-folder` using one row per sample/window. Typical columns: `sample_index`, `predicted_label`, `predicted_proba` (or one column per class with probabilities).
- A summary JSON may also be written containing model metadata, timestamp, and aggregate metrics (if ground truth is available and evaluation is requested).

### Input Data Requirements

The input CSV files should contain sensor data (accelerometer and/or gyroscope readings) with standardized formatting. The predictor expects:

- Time-series sensor data organized in windows
- Standardized feature extraction following the WearablePerMed pipeline
- Proper alignment with the window size (250 samples) used during model training

### Output Data Formats

| Format | File Extension | Use Case |
|--------|---|----------|
| NumPy Compressed | `.npz` | Default binary format, efficient storage |
| CSV | `.csv` | Human-readable, easy to inspect |

Optional outputs:
- **Label export**: Raw predicted labels in the same format as training data
- **Database export**: Predictions stored in configured database for persistence and analysis


## Troubleshooting & common issues

### Model Not Found

**Issue**: `ModelNotFoundError` when specifying a pre-trained model.

**Solutions**:
- Verify the model-type, sensor-channel, segment-body, and class-type combination is valid
- Check that models are downloaded from Hugging Face
- Use the `--model-file` argument to specify a custom local model path

### Input File Format

**Issue**: Predictor fails to read input CSV file.

**Solutions**:
- Verify CSV has correct delimiter (comma)
- Ensure sensor data has expected columns and data types
- Check file encoding (UTF-8 recommended)
- Validate against the pipeline's feature extraction output format

### Memory Issues with Docker

**Issue**: Container runs out of memory during prediction on large files.

**Solutions**:
- Increase Docker memory allocation: `docker run -m 4G ...`
- Process files in batches using separate case-id values
- Ensure input files are reasonably sized (test with smaller datasets first)

### Permission Denied in Docker

**Issue**: `Permission denied` writing to output folder.

**Solutions**:
- Ensure the `-u $(id -u):$(id -g)` flag is set correctly
- Check that output directory has write permissions
- Verify volume mounts use correct paths

### Package Installation Issues

**Issue**: Dependency conflicts when installing from source.

**Solutions**:
- Use Python 3.10 or newer: `python --version`
- Create a fresh virtual environment: `python -m venv .venv`
- Upgrade pip: `pip install --upgrade pip setuptools wheel`
- Check `requirements.txt` for pinned versions



## Testing


## Project Structure

The WearablePerMed Predictor repository is organized as follows:

```
uniovi-simur-wearablepermed-predictor/
├── src/
│   └── wearablepermed_predictor/      # Main package code
│       ├── models/                    # Model classes and implementations
│       ├── utils/                     # Utility functions
│       └── __init__.py
├── models/                             # Pre-trained model files
│   ├── RandomForest.pkl               # Random Forest model
│   └── label_encoder.pkl              # Label encoder
├── data/
│   ├── input/                         # Sample input data
│   └── output/                        # Prediction results
├── tests/                             # Unit and integration tests
├── docs/                              # Documentation
├── Dockerfile                         # Container definition
├── setup.py                           # Package configuration
├── setup.cfg                          # Version and metadata
├── requirements.txt                   # Python dependencies
├── run_predictor.sh                   # Linux/Mac launcher
├── run_predictor.bat                  # Windows launcher
├── tox.ini                            # Testing configuration
├── .coveragerc                        # Code coverage settings
└── README.md                          # Project documentation
```

### Technology Stack

- **Language**: Python 3.10+
- **Package Management**: pip, tox
- **Testing**: pytest, coverage
- **Documentation**: ReadTheDocs, Sphinx
- **CI/CD**: GitHub Actions
- **Containerization**: Docker
- **Model Formats**: Pickle (.pkl), Hugging Face Hub

### Repository Information

- **Project**: SiMuR-UO/uniovi-simur-wearablepermed-predictor
- **License**: MIT
- **Language Breakdown**: Python (83.2%), Shell (8.0%), Batch (5.4%), Dockerfile (3.4%)
- **Latest Version**: 1.18.0

## Contributing

To contribute to the WearablePerMed Predictor project:

1. **Fork** the repository on GitHub
2. **Create** a feature branch: `git checkout -b feature/your-feature`
3. **Implement** your changes with tests
4. **Run tests** locally: `tox`
5. **Commit** with clear messages: `git commit -am "description"`
6. **Push** to your fork: `git push origin feature/your-feature`
7. **Submit** a Pull Request to the main repository

### Development Setup

```bash
# Clone the repository
git clone https://github.com/SiMuR-UO/uniovi-simur-wearablepermed-predictor.git
cd uniovi-simur-wearablepermed-predictor

# Create virtual environment
python -m venv .venv
source .venv/bin/activate  # or .venv\Scripts\activate on Windows

# Install in development mode
pip install -r requirements.txt
pip install -e .

# Run tests
tox

# Build documentation
tox -e docs
```

### Notes

- This project was generated using [PyScaffold](https://pyscaffold.org/)
- The project follows Python packaging best practices
- See [CONTRIBUTING.md](https://github.com/SiMuR-UO/uniovi-simur-wearablepermed-predictor/blob/main/CONTRIBUTING.md) for detailed guidelines
- View [CHANGELOG.md](https://github.com/SiMuR-UO/uniovi-simur-wearablepermed-predictor/blob/main/CHANGELOG.md) for release history

---

