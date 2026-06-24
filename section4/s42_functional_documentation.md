---
layout: default
title: "Functional documentation"
nav_order: 2
parent: "WearablePerMed Portal"
---

# Description

In this section we are going to summarize the functionality of the WearablePerMed Portal and all its views. A workflow summary of the portal could be:

1. Create an **organization** for all participant resources and team users.
2. Create team members (**user**) inside our organization to manage cases, participants, and resources to be analyzed and visualized.
3. Create **cases** made up of **participant resources** that are grouped into **projects** to be analyzed (classified). For each case, we must define an **image** where the classifiers are implemented.
4. **Analyze** the resources to obtain **predictions**, which can be visualized in different charts.


## Getting started

By default the Portal has a unique user with the admin role to start with. It is recommended to change its default password before creating any other team member. The default credentials for this user are:

<!-- 
**username**: administrador
**password**: password
-->

To access the login view we must open this link: https://ameno.edv.uniovi.es.

![Login View](./assets/images/wearablepermed_login_view.png "Login View")

## Dashboard
The first view that you will see after login will be the Dashboard view, where the portal shows some KPIs:

- Number of users registered.
- Total cases registered.
- Total participant resources published.
- Total of jobs running to analyze resources.
- User logged at this time in the portal.
- Some papers published by the SIMUR Team Group.

![Dashboard View](./assets/images/wareablepermed_dashboard_view.png "Dashboard View")

## Organizations

By default, no organization exists, so the first step is to create your first organization from the Organization view, by clicking the burger button located at the top left of the portal toolbar. The parameters of the organization form view are:

| Name | Mandatory | Description |
| ---- | -------   | ----------- |
| Name | Yes | Name of the organization. |
| Description | No | Description of the organization. |

After creating an organization, we can:
- Edit: edit the organization values
- Remove: remove the organization and the cases attached to it.

![Organization Form View](./assets/images/wearablepermed_organization_view_form.png "Organization Form View")

## Images

The images represent the Docker artifacts where the classifier models are implemented. So we must define which images we are going to use and which classifier we will use to analyze the participant resources. By default, WearablePerMed has an image that implements some individual classifiers using RandomForest models for each segment body: Wrist, Thigh, and Hip. These images must be built and published to Docker Hub to be used by the Portal. To configure this image we can open the image view located in the case view that we will see in the next section. This configuration only needs to be executed once per case.

![Image Form View](./assets/images/wareablepermed_image_view_form.png "Image Form View")

The parameters of the form view are:

| Name | Mandatory | Description |
| ---- | -------   | ----------- |
| Name | Yes | Name of the image. This name must not be the same as the Docker image |
| Description | No | Description of the image implementing our classifiers |
| Image | Yes | This is the Docker image name. This value must be the same as the image name published under [Docker Hub Image Library](https://hub.docker.com).  |
| Version | Yes | This is the Docker image version. Also this value must be the same as the image version published under [Docker Hub Image Library](https://hub.docker.com). |
| Environment | Yes | This is the deployment environment used by the job that executes the Docker containers from the image. This value in production must be **wearablepermed**. |
| Network | Yes | This is the Docker virtual network used by the job that executes the Docker containers from the image. This value in production must be **wearablepermed-net**. |
| Command argument | Yes | This is the command executed by the classifier implemented in the image. This command must be aligned with the classifiers implemented in the image. The default arguments used by the image offered by the portal are: resource-id: unique identifier of the resource to be analyzed, model-id: unique identifier of the model to be used, user-id: unique identifier of the user who created the job to execute the classifier. |
| Arguments | Yes | These are all the argument names used by the command. These values must match with the arguments defined in the command. |

After creating an image, we can:
- Edit: edit the image values
- Remove: remove the image.

## Cases

Inside the cases view we can start to add our cases. To access this view, click on the option menu called **Cases**. By default, no case exists to group all participant resources, so the next step will be to create a case where all participant resources will use the same classifier to be analyzed. Go to the Cases option in the portal's left menu. You will see a table with all the cases created for the organization. Obviously, it will be empty.

![Case View](./assets/images/wareablepermed_case_view.png "Case View")

Inside the case view, click on the button **Add Case** to create your first case:

![Case View Form](./assets/images/wareablepermed_case_view_form.png "Case View Form")

The parameters of the form view to fill are:

| Name | Mandatory | Description |
| ---- | -------   | ----------- |
| Project | Yes | Project selection. If none exists, we must create at least one. |
| Image | Yes | Image selection. If none exists, we must create at least one. |
| Predictor Model | Yes | Classifier name implemented inside the Image selected. |
| Name | Yes | Name of the case |
| Description | No | Case Description |

As you can see, the project is a mandatory argument of any case and represents a logical group of resources. So we must create at least one project:

![Project Form View](./assets/images/wareablepermed_project_view_form.png "Project Form View")

The parameters of the project form view are:

| Name | Mandatory | Description |
| ---- | -------   | ----------- |
| Name | Yes | Name of the project. |
| Description | No | Description of the project. |

After creating a project, we can:
- Edit: edit the project values
- Remove: remove the project.

If no image exists, read the previous section about images to create one.

Like other entities, after creating a case we can:
- Edit: edit the case values
- Remove: removes the case. If the case has participants defined inside it, these will be removed in cascade, but if any of them already has predictions, the case will be deactivated instead.

## Participants

From this view we can register new participants and attach resources to them. To access this view, click on the option menu called **Participants**.

![Participant View](./assets/images/wareablepermed_participant_view.png "Participant View")

Inside this view we can start to register new participants by clicking the **Add Participant** button.

![Participant Form View](./assets/images/wareablepermed_participant_view_form.png "Participant Form View")

The parameters of this form view are:

| Name | Mandatory | Description |
| ---- | -------   | ----------- |
| Participant Id | Yes | Unique identifier of the participant. |
| First Name | No | First name of the participant. |
| Last Name | No | Last name of the participant. |
| Date of birth | No | Date of birth of the participant. |
| Age | No | Age of the participant in years. |
| Height | No | Height of the participant in meters. |
| Sex | No | Sex of the participant. |
| Skin Type | No | Skin type of the participant. |
| Notes | No | Some extra notes related to the participant. |
| Active | Yes | Flag to indicate whether the participant is active or not. Participants who are not active cannot be analyzed |

After creating a participant we can:

- Edit: edit the participant values
- Remove: remove the participant and the resources attached to it. If any of these resources have already been analyzed, the participant will be deactivated instead of removed.
- Add Resource: add a resource to the participant selected.

If we select **Add Resource**, a new view will be shown, from which we can select resources and publish them on the platform to be analyzed later.

![Resource Form View](./assets/images/wareablepermed_resource_view_form.png "Resource Form View")

From this view we can select a CSV resource with the activity from inertial sensors. These files must be in CSV format, split by commas.

## Users

This view manages the team members for each organization. Any organization must have at least one administrator in order to create new users within it.

![User View](./assets/images/wareablepermed_user_view.png "User View")

To create any user click in the button **Add User**

![User Form View](./assets/images/wareablepermed_user_view_form.png "User Form View")

The parameters of this form view are:

| Name | Mandatory | Description |
| ---- | -------   | ----------- |
| First Name | Yes | First name of the member. |
| Email | Yes | Email of the member. |
| Username | Yes | Username of the member. |
| Role | Yes | Role of the member. Possible values are: ADMIN, USER or GUEST |
| Address | No | Address of the member. |
| City | No | City of the member. |
| Phone | No | Phone of the member. |
| Language | Yes | Default language of the member |
| Notes | No | Extra notes of the member |

At any time, a user can change their password by clicking **Reset Password**, located in the user's profile menu:

![Profile View](./assets/images/wearablepermed_profile_view.png "Profile View")

## Analyze and Visualize participant resources

After creating our cases with some participants inside and attaching some resources like CSV files, we must:

- **Analyze**: this is the process where the classification of all items inside a resource into a set of classes (labels) is executed, using a machine learning model implemented in a Docker image. The result of this step will be a CSV file with all predictions, each with its own timestamp. To analyze a resource, we select the participant and, inside it, select the resource to be analyzed. If the resource does not have any prediction results yet, the Visualize button will be read-only. If some predictions have already been generated and we continue, the previous analysis will be updated with the new one.

We must know that the creation of these predictions takes a long time, since it triggers a Docker container implementing the image configured for our case. So we must wait for it to finish and persist the final results.

![Analyze View](./assets/images/wareablepermed_analyze_view.jpg "Analyze View")

- **Visualize**: After analyzing the resources and saving the predictions, we can visualize them in different formats: timeline, charts, or table list, using various graphs:

![Visualize View](./assets/images/wareablepermed_visualize_view.png "Visualize View")

