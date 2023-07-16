# Nextcloud deployment roleplay with LaymanDeploy

## Intent

I want to roleplay the deployment of Nextcloud with my Deployment tool [LaymanDeploy](https://github.com/jdmmnn/LaymanDeploy) to see how it would work out and where blackboxes are and how they could be solved.

## Requirements

- Running LaymanDeploy instance in kubernetes cluster.
- Running Database instance in kubernetes cluster.
- Running reverse Proxy for cluster.

## Deployment Process

- The User requests the deployment of a Plugin via the Frontend which is then forwarded to the Backend.
- The Backend request the plugin details form the Plugin Repository and starts the Deployment wizard where all necessary input is collected.
- The Backend creates a new Deployment, which is validated for correct inputs and used for an orchestration plan.
- The Backend deploys the Deployment to the Kubernetes cluster.
- The User receives a notification about the successful deployment.

## Roleplay

### Definitions

- **User** - The User who wants to deploy a Plugin.
- **Plugin** - The Service definition which is used to deploy a Service. It contains a helm chart abstraction with only a few necessary input values.
- **Kubernetes Cluster** - The Kubernetes Cluster where the Plugin should be deployed to and LaymanDeploy runs on.
- **Frontend** - The Frontend of LaymanDeploy which is used by the User to request a Plugin deployment.
- **Deployment Module** - The Backend Module of LaymanDeploy which is used to deploy Plugins.

### Plugin Definition

```yaml

```
