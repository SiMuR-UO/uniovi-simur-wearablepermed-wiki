---
layout: default
title: "Artefact Architecture"
nav_order: 1
parent: "System Architecture"
---

# Description
This diagram represent the artifacts relations. Actually we manage two types of articfacts:

- **Python Packages**: these artifacts are published inside [Simur Pypi Python Repository](https://pypi.org/manage/projects/){:target="_blank"}.
- **Docker Images**: these artifacts are published inside [Simur Docker Hub Repository](https://hub.docker.com/repositories/simuruo){:target="_blank"}.

This diagram groupe all artifacts in two groups::

- **Developer environment**: this environment represent all artifacts used for users with rol datascience, to pretrain and train new models based on WearablePerMed datasets. All of them are python modules published in [Pypi (The Python Package Index)](https://pypi.org/)
- **Portal environment**: this environment represent all artifacts used in the Web app WearablePerMed Portal as microservices. All of them are Docker Images published in [Docker Hub](https://hub.docker.com/).

If you need access to some of these repositories contact with [Admin Support](mailto:uo34525@uniovi.es)

<object
    data="{{ site.baseurl }}/section2/assets/images/artefactor_architecture.svg"
    type="image/svg+xml"
    width="100%">
</object>
