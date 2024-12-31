# Infrastructure as Code

## Introduction

Terraform is a tool for building, changing, and versioning infrastructure safely and efficiently.

## Traditional practices

### Pre-cloud Era

Come up with an idea -> Develop the application -> Finally, deploy on the self managed dedicated server

### Cloud Era

Come up with an idea -> Develop the applications with modern practices -> Finally, deploy the application on cloud severs with provisioning our own servers

Major changes

- The infrastructure is now provisioned by APIs.
- Servers can be created and destroyed in seconds
- Servers were long-lived and mutable, but now servers are short-lived and immutable.

### Provisioning cloud resources

Three approaches:

- GUI
- API/CLI
- IaC (**Intrastructure as Code**)

## IaC

Categories of IaC tools:

- Ad hoc scripts: Shell scripts to interact with APIs kind of IaC
- Configuration management tools(Chef, Puppet): Inclined to manage the software that is running and configuration of the infrastructure (On-Prem setups)
- Server Templating tools: Some templates and we can build in all the dependencies in the template
- Orchestration tools: Defining the deployments not the infrastructure
- Provisioning tools: Provisioning cloud resources

### Declarative vs Imperative

Declarative: Defining the end state, the tool is responsible to make API calls to meet the end state -> Provisioning tools.

Imperative: Defining the action and sequence of commands -> Configuration management tools.

### IaC provisioning tools landscape

#### Cloud Specific -> { Cloud Formation, Azure Resource Manager, Goolge Cloud Deployment Manager}

#### Cloud Agnostic -> { Terraform and Pulumi}
