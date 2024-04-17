# Setup different stages of azure aks cluster via github actions

![title](images/title.jpg)

## Purpose of this repo

Quick setup of three azure aks cluster with github actions to play around with things like azure fleet management.

## Prerequisites

- azure account - you can test it for free, see https://azure.microsoft.com
- working environment with all nessecary tools, [here is my setup guide](https://github.com/kortstie/k8s-working-environment)

## Setup resource group and service principal

### Set some variables to name our objects

Change **project** to whatever you want.

    export project=kortstie
    export resource_group=${project}rg
    export location=germanywestcentral

### Login to azure
    az login

### Create resource group
    
    az group create --name=$resource_group --location=$location

### Setup a service principal to automate things

    groupId=$(az group show --name $resource_group --query id --output tsv)
    az ad sp create-for-rbac --scope $groupId --role Contributor --json-auth

**Save** this output, we will store it now in a github secret.

### Create github repository secret

Create a github repository secret named AZURE_CREDENTIALS (go to  settings --> secrets and variables --> actions )
and store the complete output from above command in this secret.

### Create our clusters

Trigger the github workflow (.github/workflows/build-three-aks-cluster.yaml) via a new push into this repo. 

It will create three simple aks clusters in parallel, then create an image registry and connecting each cluster against the image registry.



