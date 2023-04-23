# Cluster Manifest Repository
This repository contains the configuration files for a Kubernetes cluster that manages microservices that interact with on-premise networking devices. This repository is monitored by ArgoCD, which pulls the latest changes and applies them to the cluster. When a new version of the Docker image or Helm chart is available, ArgoCD automatically updates the deployment. Both the docker image and helm chart repositories are hosted on Docker Hub.

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
Each microservice repository has a Jenkinsfile that falicitates how each microservice is built and packaged to be deployed on the cluster. When a repository has a commit pushed into a branch a webhook is sent to a Jenkins server that pulls the repository and begins the build proccess. The pipeline has the following stages:

### Pipeline Stages
#### Test
This stage runs the unit tests for the application.

#### Build Image/Chart
This stage builds the Docker image and pushes it to Docker Hub with a tag containing the commit that triggered the build webhook. This repository is then cloned into the build enviroment. The the new docker image tag is updated in the values.yaml file. If the chart version has changed, the pipeline updates the chart version in the Chart.yaml file and pushes the chart to the registry. The changes made to this repository are then commited and pushed.

#### Post Actions
A post-action that cleans the workspace and removes any generated artifacts.
