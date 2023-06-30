# LaymanDeploy

**About arc42**

arc42, the template for documentation of software and system
architecture.

Template Version 8.2 EN. (based upon AsciiDoc version), January 2023

Created, maintained and © by Dr. Peter Hruschka, Dr. Gernot Starke and
contributors. See <https://arc42.org>.

# Introduction and Goals

LaymanDeploy is intended to be a easy to use, yet powerful tool to setup a Kubernetes Cluster and orchestrate Services on it. It is intended to be used by people with little to no technical knowledge, but also by experienced users, that want to save time and effort, so it tries to abstract as much technical details as possible, while still providing the ability to customize the orchestration.

## Requirements Overview

- Provide a easy to use Interface, that abstracts all technical details that can be omitted.
- Pre-validate the orchestration through config file linting.
- Provide backup and restore capability per Service and Cluster.
- Monitor Service and Cluster health.
[Bachelor Expose with requirements](https://github.com/jdmmnn/BachelorExpose/blob/f7c4bf11c4d55d2474e41e1c8773851d4ce42cfd/template/termpaper.pdf) (in german)

## Quality Goals

- The tool should be easy to use, so that even people with little to no technical knowledge can use it.
- The orchestration should be validated before deployment, so that errors can be detected early.
- The Services and Clusters can be backed up and restored, so that data loss can be prevented.
- The Services and Clusters can be monitored, so that errors can be detected early and restarted automatically.
- The tool should be platform agnostic, so that it can be used on any platform that supports Kubernetes.
- It should provide a setup for the hardware, so that it can be assured, that the data is not accessible by unauthorized people.
- It should provide a setup for the Kubernetes cluster, so that the cluster is configured correctly for the Services.

## Stakeholders

| Role/Name       | Contact         | Expectations                                |
| --------------- | --------------- | ------------------------------------------- |
| *User*          | *\<Contact-1>*  | *Easy to use*                               |
|                 |                 | *Transparent usage with minimal user input* |
|                 |                 | *No deep technical knowledge required*      |
|                 |                 | *User Stories with bundled Services*        |
| *Administrator* | *\<Contact-2>*  | *Fast to setup*                             |
|                 |                 | *Remote monitoring*                         |
|                 |                 | *Remote deployment*                         |
|                 |                 | *Easy backup and restore*                   |
|                 |                 | *Load ready to go configuration*            |
| *Developer*     | *Jesko Dammann* | *Learning experience*                       |
|                 |                 | *working MVP*                               |

# Architecture Constraints

- Light weight
- Platform agnostic
- Kubernetes

# System Scope and Context

## Business Context

![business context](images/business-context.png)

## Technical Context

![technical context](images/technical-context.png)

**Explanation of technical interfaces**

1. **User - Web Interface**: This is the main interaction point for the user with the system. The user uses the web interface to control the backup/restore service, monitoring service, and deploy service. This involves performing tasks like starting a backup, viewing system logs, or deploying a plugin as new service(s).

2. **Web Interface - Backup/Restore Service**: This interface allows the web interface to control the Backup/Restore Service. This involves initiating a backup or restore operation, setting backup schedules, and other related tasks.

3. **Web Interface - Monitoring Service**: Through this interface, the web interface controls the Monitoring Service. This involves starting or stopping monitoring of specific services, configuring monitoring parameters, and fetching monitoring data to display to the user.

4. **Web Interface - Deploy Service**: This interface enables the web interface to control the Deploy Service. This involves deploying new services, updating existing ones, or removing services.

5. **Backup/Restore Service - Service**: This interface allows the Backup/Restore Service to fetch the necessary information from the services running on the Kubernetes cluster. This involves reading the state of a service, fetching configuration data, backing up the container, the database schema or other service-specific data necessary for backup/restore operations.

6. **Backup/Restore Service - ExternalStorage**: This interface is used by the Backup/Restore Service to store the backup data onto an external storage. This involves writing backup data, reading restore data, or performing other storage-related operations.

7. **Monitoring Service - Service**: Through this interface, the Monitoring Service monitors the services running on Kubernetes. This involves checking the service health, fetching service logs, monitoring network traffic, or other service-specific monitoring tasks.

8. **Monitoring Service - Web Interface**: This interface allows the Monitoring Service to provide status updates and logging information to the Web Interface. This involves sending real-time updates of the service health, providing log data, or other monitoring-related information.

9. **Deploy Service - GitRepository**: This interface is used by the Deploy Service to fetch the plugin definitions from a Git repository. This could involve reading the repository, fetching specific plugin definitions, or other repository-related tasks.

10. **Deploy Service - Kubernetes**: This interface enables the Deploy Service to deploy the services onto the Kubernetes cluster. This involves  orchestrating services, so creating new deployments, updating existing ones, or removing services.

11. **Service - Kubernetes**: This interface is used by the services running on Kubernetes. This could involve interacting with the Kubernetes API, running as a pod, or other Kubernetes-related tasks.

**Mapping Input/Output to Channels**

1. **User -> Web Interface (Channel: HTTP over Network)** 
    - **Input**: User commands or requests from the web interface.
    - **Output**: Responses and data from the web interface shown to the user.

2. **Web Interface -> Backup/Restore Service (Channel: Internal API Calls)**
    - **Input**: Commands from the web interface to initiate backup or restore operations, set backup schedules, etc.
    - **Output**: Responses from the backup/restore service, indicating success or failure of the operations, status updates, etc.

3. **Web Interface -> Monitoring Service (Channel: Internal API Calls)**
    - **Input**: Commands from the web interface to start or stop monitoring, configure parameters, fetch monitoring data, etc.
    - **Output**: Monitoring data, status updates, and logs from the monitoring service shown on the web interface.

4. **Web Interface -> Deploy Service (Channel: Internal API Calls)**
    - **Input**: Commands from the web interface to deploy new services, update existing ones, or remove services.
    - **Output**: Responses from the deploy service, indicating success or failure of the operations, status updates, etc.

5. **Backup/Restore Service -> Services on Kubernetes (Channel: Kubernetes API Calls)**
    - **Input**: Requests from the backup/restore service to read service state, fetch configuration data, etc.
    - **Output**: Required data for backup/restore operations from the services running on Kubernetes.

6. **Backup/Restore Service -> External Storage (Channel: Storage API Calls)**
    - **Input**: Requests from the backup/restore service to write backup data, read restore data, etc.
    - **Output**: Responses from the external storage, indicating success or failure of the storage operations.

7. **Monitoring Service -> Services on Kubernetes (Channel: Kubernetes API Calls)**
    - **Input**: Requests from the monitoring service to fetch service logs, monitor network traffic, etc.
    - **Output**: Service logs, network traffic data, and other monitoring-related data from the services running on Kubernetes.

8. **Deploy Service -> Git Repository (Channel: Git Operations over Network)**
    - **Input**: Requests from the deploy service to read the repository, fetch specific plugin definitions, etc.
    - **Output**: Plugin definitions from the Git repository.

9. **Deploy Service -> Kubernetes (Channel: Kubernetes API Calls)**
    - **Input**: Requests from the deploy service to create new deployments, update existing ones, or remove services.
    - **Output**: Responses from Kubernetes, indicating success or failure of the deployment operations.

10. **Services -> Kubernetes (Channel: Kubernetes API Calls)**
    - **Input**: Interactions of the services with the Kubernetes API, running as a pod, etc.
    - **Output**: Responses from Kubernetes, indicating the results of the operations.


# Solution Strategy

# Building Block View

## Whitebox Overall System

***\<Overview Diagram>***

Motivation  
*\<text explanation>*

Contained Building Blocks  
*\<Description of contained building block (black boxes)>*

Important Interfaces  
*\<Description of important interfaces>*

### \<Name black box 1>

*\<Purpose/Responsibility>*

*\<Interface(s)>*

*\<(Optional) Quality/Performance Characteristics>*

*\<(Optional) Directory/File Location>*

*\<(Optional) Fulfilled Requirements>*

*\<(optional) Open Issues/Problems/Risks>*

### \<Name black box 2>

*\<black box template>*

### \<Name black box n>

*\<black box template>*

### \<Name interface 1>

…

### \<Name interface m>

## Level 2

### White Box *\<building block 1>*

*\<white box template>*

### White Box *\<building block 2>*

*\<white box template>*

…

### White Box *\<building block m>*

*\<white box template>*

## Level 3

### White Box \<\_building block x.1\_\>

*\<white box template>*

### White Box \<\_building block x.2\_\>

*\<white box template>*

### White Box \<\_building block y.1\_\>

*\<white box template>*

# Runtime View

## \<Runtime Scenario 1>

-   *\<insert runtime diagram or textual description of the scenario>*

-   *\<insert description of the notable aspects of the interactions
    between the building block instances depicted in this diagram.>*

## \<Runtime Scenario 2>

## …

## \<Runtime Scenario n>

# Deployment View

## Infrastructure Level 1

***\<Overview Diagram>***

Motivation  
*\<explanation in text form>*

Quality and/or Performance Features  
*\<explanation in text form>*

Mapping of Building Blocks to Infrastructure  
*\<description of the mapping>*

## Infrastructure Level 2

### *\<Infrastructure Element 1>*

*\<diagram + explanation>*

### *\<Infrastructure Element 2>*

*\<diagram + explanation>*

…

### *\<Infrastructure Element n>*

*\<diagram + explanation>*

# Cross-cutting Concepts

## *\<Concept 1>*

*\<explanation>*

## *\<Concept 2>*

*\<explanation>*

…

## *\<Concept n>*

*\<explanation>*

# Architecture Decisions

# Quality Requirements

## Quality Tree

## Quality Scenarios

# Risks and Technical Debts

# Glossary

| Term        | Definition        |
| ----------- | ----------------- |
| *\<Term-1>* | *\<definition-1>* |
| *\<Term-2>* | *\<definition-2>* |
