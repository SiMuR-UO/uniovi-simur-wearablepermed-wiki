---
layout: default
title: "Project Management"
nav_order: 1
parent: "System Artifacts"
---

# Description

In this section we explain how to start, code, fix, and debug any project. We follow the mono-repository pattern to implement all our repos. Currently we manage these repositories:

- **Utils Repository**: this repository implements all the steps used in the pretrain pipeline.
- **Pipeline Repository**: This repository automates the execution of all the steps implemented in the **Utils Repository**.
- **Machine Learning Repository**: This repository is used to train the machine learning models using the datasets created with the **Pipeline Repository**.
- **Machine Learning Pipeline Repository**: This repository is used to test and calculate the metrics from the models created with the **Machine Learning Repository**.
- **Predictor Repository**: This repository implements the classification of any raw dataset using one of the previous models from the **Machine Learning Repository**.

## Getting Started: Project Scaffolding

To scaffold the project repository we will use this tool: [PyScaffold](https://pyscaffold.org/en/stable/). Follow these steps to install the tool and extensions:

- Install the PyScaffold tool on your host by executing this command:
    ```
    $ pip install --upgrade pyscaffold
    ```

- By default this tool does not use Markdown as the documentation language, so to use this standard documentation language we must install an extension called **pyscaffoldext-markdown**, executing this command:
    ```
    $ pip install pyscaffoldext-markdown
    ```

- After this, we can already scaffold our project using the **--markdown** argument, executing this command:
    ```
    $ putup --markdown uniovi-simur-wearablepermed-utils -p wearablepermed_utils \
        -d "Uniovi Simur WearablePerMed Utils." \
        -u https://github.com/SiMuR-UO/uniovi-simur-wearablepermed-utils.git 
    ```

- After creating the default project, and before coding anything, we must create a new virtual environment inside the newly scaffolded project and activate it:
    ```
    $ python -m venv .venv
    $ source .venv/bin/activate
    ```

- Install an automated project management tool called [tox](https://tox.wiki/en/4.17.2/), integrated with PyScaffold. We will use this tool to build and publish the Python modules in PyPI:
    ```
    $ pip install --upgrade tox
    ```

- Finally, after this we can install our particular Python dependencies, such as Pandas, Scipy, Matplotlib, etc., and start coding:
    ```
    $ pip install -U numpy pandas scipy openpyxl matplotlib
    ```

- Don't forget to create the dependency file called **requirements.txt** after installing any dependency inside your project, by executing this command:
    ```
    $ pip freeze > requirements.txt
    ```

## Project Code and Debugging
If we clone the repository for the first time to add a new feature or fix a bug, we must follow these steps:

- Clone the repository. All repositories are public, so no credentials are needed. For example, to clone the Utils Repository execute this command:
    ```
    $ git clone https://github.com/SiMuR-UO/uniovi-simur-wearablepermed-utils.git
    ```

- Create a separate virtual environment inside the newly cloned repository:
    ```
    $ python -m venv .venv
    $ source .venv/bin/activate
    ```

- Install project dependencies from the **requirements.txt** file:
    ```
    $ pip install -r requirements.txt
    ```

- Install the module locally for debugging:
    ```
    $ pip install -e .
    ```

- Save the project requirements, only if you add new dependencies to the project:
    ```
    $ pip freeze > requirements.txt
    ```

## Project Management

After coding and fixing any project, we can execute commands to: test, clean, build, generate documentation, or publish your library to [PyPI](https://pypi.org/). Don't forget to update the library version in the **setup.cfg** project build file.

We must manually update the versions of these projects and their dependencies, if they exist, from the **setup.cfg** file. Please review the relationships between these artifacts (repos) in this [diagram](../section2/) to know which repo and dependency versions we must update.

After updating these versions, the commands offered by the **tox** tool are these:

- **clean**: to remove any build and temporary files created previously in any build.
- **build**: to compile our Python module to be published.
- **docs**: to create technical documentation for our project.
- **publish**: to publish our built Python module to PyPI. We must be logged into the Simur PyPI account (with the credentials installed on your computer) to publish any repo. These are specifically the commands to be executed using tox:

    ```
    $ tox
    $ tox -e clean
    $ tox -e build
    $ tox -e docs
    $ tox -e publish -- --repository pypi
    ```

