# Questions for Saarstedt  
- the inteded deployment tool does almost everything that needs to be done, concerning orchestration.  
	- helm charts just have to be filled with values, which can easily be done through helmfile  
	- helmfile itself wrappes helm charts perfectly and can be used as the base for the plugins system

- i'm questioning the validity of my work and whether or not it is extensive enough to warrant a bachelors degree.
	- currently the only thing im doing is learning kubernetes, helm and helmfile and wrapping those into a usable interface with a few quality of life improvements.  
	- What should be the scope? is learning to use and showing this through the implementation of a rudimentary wrapper enough?  
	- the deployment process (the heart of the application) itself is already practically done (through the usage of helmfile) and logic wise, there is basically nothing for me to do, besides defining the plugins and writing a few of them.
	- i would only write a basic backend that gets a few inputs through a frontend, fills the values.yaml file for each helmfile and use helmfiles capability to deploy them.  
		- obviously the frontend needs to be developed, but that does not seem as too big a task.  
	- for the setup i'm only writing a basic (ansible playbook) script, that configures a server, sets up kubernetes through k3s and deploys the service that only wrappes capability together.

- an almost equivalent alternative already exists: [stackspin](https://www.stackspin.net/)
- similarily [soverign-workplace](https://software.opencode.de/project/351) a project by the BMI for basically the same.

## Unique Selling Point vs stackspin:
- extensible Plugin System.
- easier setup (through gui)
- Everything else is already done in [stackspin](https://www.stackspin.net/)

### missing
- SSO
- Monitoring

### similarities
- usage of kubernetes
- dashboard
- deployment of services (only predefined ones)
  - other apps have to be manually installed but can be integrated into the system
- backup and restore

## Unique Selling Point vs sovereign-workplace:



## Overview of what I intend to develop

### 1. LaymanSetup  
- A tool for configuring a server  
	- make it secure (iptables, ssh, user_access)  
	- enable automatic updates  
- Install and configure k3s  
- deploy my service

### 2. LaymanDeploy  
- Is a wrapper for helmfiles  
- provides a webfrontend where  
	- helmfiles can be completed with necessary user inputs  
	- helmfiles gets triggered to deploy those applications  
- helmfile provides a deployment status  
- validates the deployment  
	- confirm that helmfile is valid  
	- that the correct database is used  
- accesses a helmfile registry to load available files  
- has backup and restore capability  
	- uses the plugin definition to define what data to save  
	- has a external data adapter where to save the encrypted data to

### 3. Plugins  
- writing a few Plugins for services like Nextcloud, paperless-ngx, fireflyiii, jelly-fin, plex, homeassistant, etc