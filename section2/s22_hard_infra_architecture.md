---
layout: default
title: "Hard Infrastructure"
nav_order: 2
parent: "System Architecture"
---

# Description
This diagram represent the hardware infrastructure that SIMUR group offer to its projects

- **Server Node**: this node represent the servers where we train our models. These nodes manage:
    - **Input data**: folder where the input datasets lieves. These represent the raw data used in the process of the pre-train steps. These files **are synchronized**.
    - **Auxiliar data**: folder where the auxiliar datasets leaves. Represents temporal datasets used in the process of the pre-train steps. **These files **are not synchronized**.
    - **Output data**: folders where final pre-trained datasets and trained models leaves. These files **are synchronized**.

- **NAS Nodes**: This node represent the servers where the input and output files are synchronized and mirror all the time to be preserved.

If you need access to some of these repositories contact with [Admin Support](mailto:uo34525@uniovi.es)

<object
    data="{{ site.baseurl }}/section2/assets/images/hard_infra_architecture.svg"
    type="image/svg+xml"
    style="max-width: 1200px; width: 100%;">
</object>
