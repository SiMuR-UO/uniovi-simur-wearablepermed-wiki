---
layout: default
title: "Project Management"
nav_order: 1
parent: "Artefactos del sistema"
---

# Description

In this section we explain how start any project, code, fix and debug. We follow the patterns mono-repository to implement all repos. Actually we manage these respository:

- **Utils Repository**: this repository implemente all steps used in the pretrain step.
- **Pipeline Repository**: This repository automates the execution of all utils steps repository.
- **Machine Learning Repository**: This repository is used to train the machines learning models using the datasets creates with the pipeline repository.
- **Machine Learning Pipeline Repository**: This repository is used to test and calculate the metrics from the previous models created with the Machine Learning Repository.

## Get starting: Project Schaffolding

To schaffold the project repository we will use this tool [PyScaffold](https://pyscaffold.org/en/stable/). Follow this steps to install the tool and extensions:

- Install PyScaffold tool in your host executing this command:
    ```
    $ pip install --upgrade pyscaffold
    ```

- By default this tool not use markdown as default documentation language, so to use this standard documentation language we must install an extension called **pyscaffoldext-markdown**, executing this command:
    ```
    $ pip install pyscaffoldext-markdown
    ```

- After this, already we can scaffold our project using the argument **--markdown** and this tool executing this command:
    ```
    $ putup --markdown uniovi-simur-wearablepermed-utils -p wearablepermed_utils \
        -d "Uniovi Simur WearablePerMed Utils." \
        -u https://github.com/SiMuR-UO/uniovi-simur-wearablepermed-utils.git 
    ```

- After create the default project and before code anything, we must create a new virtual environment inside the just project scaffolded and active it:
    ```
    $ python -m venv .venv
    $ source .venv/bin/activate
    ```

- Install a tool to  automation project manager called [tox](https://tox.wiki/en/4.17.2/) integrated with PyScaffold. We will use these tool to build and publish the python modules in Pypi:
    ```
    $ pip install --upgrade tox
    ```

- Finally, after this we can install our particularly python dependencies like: Pandas, Scipy, Matplotlib, etc, and start to code:
    ```
    $ pip install -U numpy pandas scipy openpyxl matplotlib
    ```

- Don't forget to create the dependency file called **requirements.txt** after install any dependency inside your project, executing this command:
    ```
    $ pip freeze > requirements.txt
    ```

## Project Code and Debugging
If we clone the repository the first time to add a evolutive or correct some fix we must follow this steps:

- Clone the repository. All repositories are public so any credentials are needed. For example to clone the Utils Repository execute this command:
    ```
    $ git clone https://github.com/SiMuR-UO/uniovi-simur-wearablepermed-utils.git
    ```

- Create a particular virtual environment inside the recent cloned repository:
    ```
    $ python -m venv .venv
    $ source .venv/bin/activate
    ```

- Install project dependencies from **requirements.txt** file:
    ```
    $ pip install -r requirements.txt
    ```

- Install module locally for debugg
    ```
    $ pip install -e .
    ```

- Save project requirements, only if you add new dependencies in the project:
    ```
    $ pip freeze > requirements.txt
    ```

## Project management

After code and fix any project we can execute commands for: test, clean, build, generate documentation or publish your library in [PyPI](https://pypi.org/). Don't forget update the version library from **setup.cfg** project build file:.

We must manually update the versions of these projects and there dependencies if exist from **setup.cfg** file. Please review the relation of these artifacts (repos) from this [diagram](../section2/) to now what repo and dependencies versions we must to update.

After update these versions, the command offered by **tox** tool are these ones:

- **clean**: to remove any build and temporal file created previously in any build.
- **build**: to compile our python module to be published.
- **docs**: to create technical documentation for our project.
- **publish**: to publish our build python module in PyPI. We must be logged in the Simur PyPI account (have the credentials installed in your computer) to publish any repo. These are squecially the commands to be executed using tox:

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