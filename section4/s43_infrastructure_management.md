---
layout: default
title: "Infrastructure management"
nav_order: 3
parent: "WearablePerMed Portal"
---

# Description

As we explained in previous sections, the system infrastructure follows the microservices pattern, so all services and microservices are packaged as Docker images. The business services are published under a particular [Simur Account](https://hub.docker.com/repositories/simuruo) Docker registry. So any business microservice update or fix will result in a new Docker image published under this Simur Docker account.

We can see in the screenshot all the Docker images published by the system:

![Docker Hub Services](./assets/images/docker_hub_services.png "Docker Hub Services")

The rest of the services that implement the infrastructure—databases, security, or proxy—are also Docker images, but published under other public account Docker registries, such as [Docker Hub](https://hub.docker.com/) or the [quay.io Red Hat registry](https://quay.io/).

To publish any business microservice under this particular Simur Docker account on [Docker Hub](https://hub.docker.com/repositories/simuruo), you will need credentials. Send an email to [Antonio Lopez](mailto:amlopez@uniovi.es) to obtain credentials.

Also, on the node where the services are deployed, we can use the Docker CLI at any time to manage any container running inside it. The node also offers a web application called [Portainer](https://www.portainer.io/) under the Community Edition, which can be accessed from this link: [Portainer Portal](https://localhost:9443).

![Portainer Services](./assets/images/portainer_service.png "Portainer Services")

If you need access to this app, please send an email to [Miguel Salinas Gancedo](mailto:uo34525@uniovi.es) to obtain credentials.
