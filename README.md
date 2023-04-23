# Cluster Manifest Repository
This repository contains the configuration files for a Kubernetes cluster that manages microservices that interact with on-premise networking devices. It is updated automatically by a Jenkins pipeline. This repository is monitored by ArgoCD, which pulls the latest changes and applies them to the cluster. When a new version of the Docker image or Helm chart is available, ArgoCD automatically updates the deployment. Both the docker image and helm chart repositories are hosted on Docker Hub.

## Prerequisites
* Jenkins Server
* Docker registry account
* Git repository for the chart
* Kubernetes Cluster with ArgoCD deployed

## Component Repositories
* [Network Web](https://github.com/SteffenSenchyna/network-web)
* [Network API](https://github.com/SteffenSenchyna/network-api)
* [Syslog Server](https://github.com/SteffenSenchyna/syslog)
* [SNMP Server](https://github.com/SteffenSenchyna/snmp)
* [Network Cron Jobs](https://github.com/SteffenSenchyna/network-cron)

## Jenkins Pipeline
Each microservice repository has a Jenkinsfile that falicitates how each microservice is built and packaged to be deployed in the cluster. What a repository has a commit pushed into a branch a webhook is sent to a Jenkins server that pulls the repository and begins the build proccess. The pipeline has the following stages:

### Pipeline Stages
#### Test
This stage runs the unit tests for the application.

#### Clone Chart Repo and Build Image/Chart
This stage clones this chart repository and builds the Docker image for the application. If the chart version has changed, the pipeline updates the chart version in the Chart.yaml file and pushes the chart to the registry. It also updates the values.yaml file with the new image tag and commits and pushes the changes to the repository. 

#### Post Actions
The pipeline includes a post-action that cleans the workspace and removes any generated artifacts.
