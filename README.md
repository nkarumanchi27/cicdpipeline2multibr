# Multibranch CI/CD Pipeline repository for a Dockerized Hello World application
This repository have everything you needed to build, deliver and deploy a sample Hello World application on to the Google Kubernetes Engine. It has two primary branches "stage, master". While the development is done onto the stage branch, production ready code is merged onto the Master branch through proper GitHub pull requests, review and approval process. There is a webhook setup between GitHub and Jenkins server to detect new branches, changes in branches and trigger pipeline runs. There is also, a Jenkinsfile in each branch with the necessary "When" conditions to skip some of the build stages to support a Jenkins based multibranch pipeline setup.

When it comes to the deployment aspects, this Jenkins based multistage build job supports end-to-end ci/cd pipeline for the "stage" branch code onto its own GKE cluster "CKE-Stage-Cluster", and another end-to-end CI/CD pipeline deployment for the "master" branch onto to its own GKE cluster " GKE-Production-cluster". Both these Kubernetes clusters are created and managed through IaC using Terraform and the associated repository URL.


# More details on the files present in this repository
 - Server.js, package.json: These are the two files implementing the application functionality.
 - Jenkinsfile : This file has various stages & steps for the build, deliver and deployment of the applicaiton. It also has the necessary conditions inplace to skip some of the build steps based on the Github branch.
 - Dockerfile: This file has necessary steps to create a docker image for the application.
 - deployment.yaml : This file has necessary code to create kubernetes resources ( Deployment and Servie).


# Pre-requisites

1. A Jenkins server instance with necessary plugins installed & cridentials configured.
2. A devlopment server instance with Docker engine installed and configured.
3. Docker hub account and cridentials to push images.
4. GKE Cluster with necesary service accounts,roles created, configured to deploy the application. 

# CI/CD Pipeline workflow

1. Create a multibranch pipeline job on to your jenkins server instance.
    - Branch source: Git,project repository: THis repository URL, Cridentials: GitHub Cridentials.
    - Build configuration Mode: by Jenkinsfile, Script path: Jenkinsfile.
    - Scan multibranch pipeline Triggers: Scan by webhook.
2. Program the GitHub Webhook with the jenkins URL " JENKINS_URL/multibranch-webhook-trigger/invoke?token=[Trigger token]" to push repo/branch change notification payloads.
3. add the necessary branch protections to your GitHub repository. Current implementation require a pull request for merging branches with atleast 1 approval to merge.
4. Preapre both the Stage and production GKE clusters, Docker machine, Jenkins to Docker integration.
5. Make some changes onto the files from "stage" branch and commit them. You can Edit the "hello world" text from the applicaiton or can edit something on this readme file.
6. This will trigger a CI/CD pipeline on the "stage" branch, which will pull the source from github, build a docker image, push the docker image onto Docker Hub, deploy the built image onto GKE Stage cluster.
7. Also, on the GitHub screen you will see an option to create a pull request for merging these changes onto "master" branch. 
8. When you create that pull request, the collaborators/reviewers of this repository will be notified to review and approve the pull request.
9. when the pull request is approved,merged onto master, it will trigger another CI/CD pipeline , this time on the master branch.
10. This master branch pipeline also, will pull the source from GitHub, build a Docker image,  Push the image onto Docker Hub, Deploy the image onto GKE production cluster. 
