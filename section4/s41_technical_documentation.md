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

