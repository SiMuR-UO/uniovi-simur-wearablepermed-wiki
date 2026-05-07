---
layout: default
title: "Utils Repository"
nav_order: 2
parent: "Artefactos del sistema"
---

# Descripción

This repository implement all steps that must be executed sequentially to create the pretrain datasets from raw segment bodies datasets obtained from these inertial sensors, located in:

- Wrist
- Thight
- Hip

## Schaffolding
The schaffolding of the project is create using this tool [PyScaffold](https://pyscaffold.org/en/stable/):

- By default this tool not use markdown as default documentation language, so ti use this standard documentation format we must install a pyscaffoldext extension called **pyscaffoldext-markdown**, executing this command:
    ```
    $ pip install pyscaffoldext-markdown
    ```

- After this we can scaffold our project using the argument **--markdown** executing this command:
    ```
    $ putup --markdown uniovi-simur-wearablepermed-utils -p wearablepermed_utils \
        -d "Uniovi Simur WearablePerMed Utils." \
        -u https://github.com/SiMuR-UO/uniovi-simur-wearablepermed-utils.git 
    ```

- After create the default project, we must create a new virtual environment inside the project and active it:
    ```
    $ python -m venv .venv
    $ source .venv/bin/activate
    ```

- Install and upgrade tox automation project manager. We will use these tools to build and publish the python modules in Pypi:
    ```
    $ pip install --upgrade tox
    ```

- Finally, afte this we can install out particulary python dependencies like: Pandas, Scipy, Matplotlib, etc:
    ```
    $ pip install -U numpy pandas scipy openpyxl matplotlib
    ```

## Code and Debugging

Install library modules:
```
$ pip install -r requirements.txt
```

Install module locally for debugg
```
$ pip install -e .
```

Save project requirements:
```
$ pip freeze > requirements.txt
```

## Project management

Project commands for: test, clean, build, generate documentation or publish your library in pypi repository
Don't forget update the version library from **setup.cfg** project build file:

```
$ tox
$ tox -e clean
$ tox -e build
$ tox -e docs
$ tox -e publish -- --repository pypi
```

## Testing

- Convert binary file to csv:
     ```
     $ sensor_bin_to_csv \
     --bin-file /home/miguel/git/uniovi/simur/uniovi-simur-wearablepermed-utils/data/input/PMP1053/PMP1053_W1_C.BIN \
     --csv-file /home/miguel/git/uniovi/simur/uniovi-simur-wearablepermed-utils/data/input/PMP1053/PMP1053_W1_C.csv
     ```

- Segment all sensor csv files for each participant and correct deviation if exists:
     For participant csv files without some deviation error
     ```
     $ csv_to_segmented_activity \
     --csv-file /home/miguel/git/uniovi/simur/uniovi-simur-wearablepermed-utils/data/input/PMP1053/PMP1053_W1_M.csv \
     --excel-activity-log /home/miguel/git/uniovi/simur/uniovi-simur-wearablepermed-utils/data/input/PMP1053/PMP1053_RegistroActividades.xlsx \
     --body-segment M \
     --output /home/miguel/git/uniovi/simur/uniovi-simur-wearablepermed-utils/data/input/PMP1053/PMP1053_W1_seg_M.npz
     ```

     For participant csv files with some deviation error
     ```
     $ csv_to_segmented_activity \
     --csv-file /home/miguel/git/uniovi/simur/uniovi-simur-wearablepermed-utils/data/input/PMP1053/PMP1053_W1_M.csv \
     --excel-activity-log /home/miguel/git/uniovi/simur/uniovi-simur-wearablepermed-utils/data/input/PMP1053/PMP1053_RegistroActividades.xlsx \
     --body-segment M \
     --output /home/miguel/git/uniovi/simur/uniovi-simur-wearablepermed-utils/data/input/PMP1053/PMP1053_W1_seg_M.npz \
     --sample-init 16088120 \
     --start-time 17:40:00
     ```

- Window segmented files for convolution model likes
     The argument **include-not-estructure-data** can be included if you want add not estructure data
     ```
     $ segmented_activity_to_stack \
     --npz-file /home/miguel/git/uniovi/simur/uniovi-simur-wearablepermed-utils/data/input/PMP1053/PMP1053_W1_seg_M.npz \
     --crop-columns 1:7 \
     --window-size 250 \
     --window-overlapping-percent 50 \
     --output /home/miguel/git/uniovi/simur/uniovi-simur-wearablepermed-utils/data/input/PMP1053/PMP1053_W1_tot_M.npz
     ```

- Extract features from windowed segmented files for random forest model likes
     ```
     $ stack_to_features \
     --stack-file /home/miguel/git/uniovi/simur/uniovi-simur-wearablepermed-utils/data/input/PMP1053/PMP1053_W1_tot_M.npz \
     --output /home/miguel/git/uniovi/simur/uniovi-simur-wearablepermed-utils/data/input/PMP1053/PMP1053_W1_tot_M_features.npz∫
     ```

- Partial aggregation for each participant datasets
     ```
     $ aggregate_windows_features \
     --dataset-folder /home/miguel/git/uniovi/simur/uniovi-simur-wearablepermed-utils/data/input/PMP1003 \
     --ml-models ESANN,RandomForest \
     --ml-sensors thigh,hip,wrist \
     --output-folder /home/miguel/git/uniovi/simur/uniovi-simur-wearablepermed-utils/data/input/PMP1003
     ```

- Total aggregation for all participant datasets to train models
     ```
     $ model_aggregation \
     --dataset-folder /home/miguel/git/uniovi/simur/uniovi-simur-wearablepermed-utils/data/input \
     --output-folder /home/miguel/git/uniovi/simur/uniovi-simur-wearablepermed-utils/data/output \
     --case-id case_sample
     ```