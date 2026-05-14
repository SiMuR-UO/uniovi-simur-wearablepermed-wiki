---
layout: default
title: "Technical documentation"
nav_order: 1
parent: "WearablePerMed Portal"
---

# Description

This section it's about the architecture followed by WearablePerMed Portal. The architecure used to implement this solution is a polyglote microservices pattern. The principal characteristics are:

- A Secure solution using the standard OAuth 2.0.
- A Multitenant solution to support several organizations at the same time.
- A Microservice architecture using Docker Containers.
- Decouple solution using external docker images to implement classifiers.
- A Rich Internet Application (RIA) for UI.

## Microservices

The system follow a polyglote microservices pattern because we use three diferent languages to be implemented:

- **Java (SpringBoot)**: to implement the bussiness logic of the system
- **Python**: to implement the machine learning logic of the system
- **Typescript, html and scss**: to implement the ui logic of the system

We can divide these services in two groups:

- **Infrastructure services**: these services support the infraestrcucture like: state, security and access control to the system. The next table show the list of all infrastructure microservices:

| Name                           | Image                                               | Version | Description |
| ------------------------------ | --------------------------------------------------- | ------- | ------------ |
| wearablepermed-keycloak        | quay.io/keycloak/keycloak                           | 26.0.0  | [Keycloak (Red-Hat)](https://www.keycloak.org/) implementing the Indentity and Access Management implemented by Red-Hat |
| wearablepermed-keycloak-db     | postres                                             | 15      | [PostgreSQL (SQL Database)](https://www.postgresql.org/) used by Keycloak to manage the state of the service |
| wearablepermed-mongodb         | mongo                                               | 7.0.14  | [(No SQL Database)](https://www.mongodb.com/) to manage the system microservices state |
| wearablepermed-proxy           | haproxy                                             | latest  | [haproxy proxy](https://www.haproxy.com/) to redirect any external request to inside simur nodes |

- **Business microservices**: this microservices implement the businnes domain of our system using the languages already explained in before chapters. The next table show the list of all microservices:


| Name                           | Image                                               | Version | Language          |  Description |
| ------------------------------ | --------------------------------------------------- | ------- | ----------------- | ------------ |
| wearablepermed-backend-gateway | simuruo/uniovi-simur-wearablepermed-backend-gateway | 1.0.0   |  SpringBoot (Java) | Gateway pattent to redirect any requets to microservice |
| wearablepermed-backend-job | simuruo/uniovi-simur-wearablepermed-backend-job         | 1.3.0   |  SpringBoot (Java) | Implement the backend Job that execute the precdictor as a docker container when try to analyze any resource integrated with Docker|
| wearablepermed-backend-organization | simuruo/uniovi-simur-wearablepermed-backend-organization | 1.0.0   | SpringBoot (Java) | Implement the backend system business model infrastructures like: organizations, projects, images, cases of the system |
| wearablepermed-backend-participant | simuruo/uniovi-simur-wearablepermed-backend-participant | 1.4.0   | SpringBoot (Java) | Implement the backend system business model infrastructures like: participants, resources and predictions of the system|
| wearablepermed-backend-security | simuruo/uniovi-simur-wearablepermed-backend-security | 1.0.0   |  SpringBoot (Java) | Implement the backend security model infrastructures like: users, roles and integrate with Keycloak |
| wearablepermed-frontend-ui | simuruo/uniovi-simur-wearablepermed-frontend-ui | 1.3.0   | Angular (typescript, html, scss) | Implement the frontend inetrafce to interact wit the system throw all backend microservices |
| uniovi-simur-wearablepermed-backend-job-predictor | simuruo/uniovi-simur-wearablepermed-backend-job-predictor | 1.21.0  | the docker image implementing all machine models to be used when classifie. This docker image you the last python library called [uniovi-simur-wearablepermed-predicto](https://pypi.org/project/uniovi-simur-wearablepermed-predictor/). This image is instanciated under demand where try to analyze a participant resource, so the container created by docker its a job that will be removed when this service finalize its work, so only exists during this time interval. This image is instanciated by the service called **wearablepermed-backend-job** |

This capture show all microservices inside the docker stack called **uniovi-simur-wearablepermed-infrastructure**:

![Docker Services](./assets/images/wearablepermed_docker_services.jpg "Docker Services")

Finally we can see in this deployment diagram all services and there relations:

![WareablePerMed Services](./assets/images/wareablepermed_services.jpg "WareablePerMed Services")

