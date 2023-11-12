# DevOps Project

This project is designed to implement a robust continuous integration and deployment pipeline for an application. It utilizes widely adopted tools in DevOps, such as GitHub/GitLab, Docker, Jenkins, and Kubernetes. The goal is to streamline development processes, ensuring the integration and deployment through the efficient orchestration of these tools.

## Table of Contents

- [Installation](#Installation)
- [Docker](#Docker)
- [Dockerhub](#Dockerhub)
- [Jenkins](#Jenkins)
- [Kubernetes](#Kubernetes)

## Installation
steps to clone the project and installing the required dependencies 
```
git clone https://github.com/azizz98/projet_back
cd projet_back
npm i
```

## Docker
we will use the docker file included in the project to build a docker image for the project 
```
docker build -t project .
```
we can also run a docker container based on that image to test it locally
```
docker run -p 8084:8084 project
```

## Dockerhub
sign up on dockerhub and get login credentials, we will use those later on to setup the pipeline in jenkins.
```
sign up here -> https://hub.docker.com
```

## Jenkins
the project includes the pipeline code in the Jenkinsfile. In the jenkins dashboard, create a new pipeline and set the repo uri of the project, then navigate to settings->credentials and create new credentials under the tag name 'dh_cred' and use the creds of your dockerhub account. 

if everything is setup correctly, the pipeline should be working and once we run it manually or make a change in the project repo on github, we can notice the deployment of the project on dockerhub dashboard.
PS: the pipeline checks for changes in the repo every 5 minutes, we can change it by editing the value of the pollSCM in the Jenkinsfile
```
pipeline {
    agent any
    triggers { pollSCM('*/5 * * * *') #HERE
    }
    environment {
        DOCKERHUB_CREDENTIALS = credentials('dh_cred')

    }
```

## Kubernetes
kubernetes deployment and services yaml files are included in the project in the k8s folder, we will use kubectl to apply them
```
kubectl apply -f back-deployment.yaml --context minikube
kubectl apply -f services.yaml
```
we can now verify the status of the deployments and the services
```
kubectl get deployments
kubectl get services
```
