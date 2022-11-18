# CI/CD Pipeline repository for a Dockerized Hello World application
This repository have everything you needed to build, deliver and deploy a sample Hello World application on to the Google Kubernetes Engine. It has two primary branches Stage, master. While the development is done onto the stage branch, production ready code is merged onto the Master branch through proper GitHub pull requests, review and approval process. There is a Jenkins file in each branch with the necessary "When" conditions to skip some of the build stages to support a Jenkins based multibranch pipeline setup.

When it comes to the deployment aspects, this multistage build supports end-to-end ci/cd pipeline for the "stage" branch code onto its own GKE cluster "CKE-Stage-Cluster", and another end-to-end CI/CD pipeline deployment for the "master" branch onto to its own GKE cluster " GKE-Production-cluster". Both these Kubernetes clusters are created and managed through IaC using Terraform and the associated repository URL.


# More details on the files present in this repository
 - Server.js, package.json: These are the two files implementing the application functionality.
 - Jenkinsfile : This file has various stages & steps for the build, deliver and deployment of the applicaiton. It also has the necessary conditions inplace to skip some of the build steps based on the Github branch.
 - Dockerfile: This file has necessary steps to create a docker image for the application.
 - deployment.yaml : This file has necessary code to create kubernetes resources ( Deployment and Servie).


# Pre-requisites

1. A Jenkins server instance with necessary plugins installed.
2. A devlopment server instance with Docker engine installed and configured.
3. GKE Cluster with necesary service accounts and configurations to deploy the application. 

# CI/CD Pipeline workflow

1. Create a multibranch pipeline on to your jenkins server instance.  
