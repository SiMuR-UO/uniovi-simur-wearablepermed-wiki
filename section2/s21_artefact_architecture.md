---
layout: default
title: "Artefact Architecture"
nav_order: 1
parent: "System Architecture"
---

# Description
This diagram represents the relationships between artifacts. Currently we manage two types of artifacts:

- **Python Packages**: these artifacts are published inside [Simur Pypi Python Repository](https://pypi.org/manage/projects/){:target="_blank"}.
- **Docker Images**: these artifacts are published inside [Simur Docker Hub Repository](https://hub.docker.com/repositories/simuruo){:target="_blank"}.

This diagram groups all artifacts into two groups:

- **Developer environment**: this environment represents all artifacts used by users with the data science role, to pretrain and train new models based on WearablePerMed datasets. All of them are Python modules published in [Pypi (The Python Package Index)](https://pypi.org/).
- **Portal environment**: this environment represents all artifacts used in the WearablePerMed Portal web app as microservices. All of them are Docker Images published in [Docker Hub](https://hub.docker.com/).

If you need access to any of these repositories, contact [Admin Support](mailto:amlopez@uniovi.es).

<object
    data="{{ site.baseurl }}/section2/assets/images/artefactor_architecture.svg"
    type="image/svg+xml"
    width="100%">
</object>
