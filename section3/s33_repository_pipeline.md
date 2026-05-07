---
layout: default
title: "Pipeline Repository"
nav_order: 3
parent: "Artefactos del sistema"
---

# Description

This section explain how we can execute all steps implemented in the previous python module called **utils** in a squencial way implemented using a pipeline and all arguments this pipeline offer to execute diferent user cases. The result of these repository are pretrain datasets to be used to traing models using later repositories.

## Testing

Execute one pipeline from this command:
```
python3 main.py \
    --verbose \
    --execute-steps 2,3,4,5,6 \
    --dataset-folder /home/miguel/git/uniovi/simur/uniovi-simur-wearablepermed-utils/data/input \
    --participants-missing-file /home/miguel/temp/wearablepermed_pipeline/missing_end_datetimes.csv \
    --crop-columns 1:7 \
    --window-size 250 \
    --window-overlapping-percent 50 \
    --ml-models ESANN,RandomForest \
    --ml-sensors thigh,hip,wrist \
    --output-case-folder /home/miguel/git/uniovi/simur/uniovi-simur-wearablepermed-utils/data/output \
    --case-id case_sample
```