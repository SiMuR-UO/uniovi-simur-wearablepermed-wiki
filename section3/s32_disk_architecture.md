---
layout: default
title: "Disk Architecture"
nav_order: 2
parent: "Arquitectura del sistema"
---

# Descripción
This diagram represent the disk architecture. Actually we manage tree node in this architecture:

- **Server Node**: this node represent the servers where we train our models. These nodes manage:
    - Input data: folder where the input datasets lieves. These represent the raw data  used in the process of the pre-train steps. These are syncronized
    - Auxiliar data: folder where the auxiliar datasets lieves. Represents temporal datasets used in the process of the pre-train steps. These are not syncronized.
    - Output data: cases folders and models subfolders, where these datasets and models lieves. Represent the datasets where the final pre-train datasets and models are saved

- **NAS Nodes**: These nodes represent the servers where some input and output datasets are syncronized and mirrow all the time to be preserved.

If you need access to some of these repositories contact with [Admin Support](mailto:uo34525@uniovi.es)

<object data="{{ site.baseurl }}/section3/assets/images/disk_architecture.svg" type="image/svg+xml" width="100%"></object>