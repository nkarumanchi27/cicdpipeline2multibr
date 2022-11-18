# Multibranch CI/CD Pipeline Repository for a Dockerized Hello World Application
This repository has everything you needed to build, deliver, and deploy a sample Hello World application on to the Google Kubernetes Engine Clusters. It has two primary branches "stage, master". While the "stage" branch is used to develop and test the application, production ready code is merged onto the Master branch through proper GitHub pull requests, review and approval process. There is a GitHub webhook setup between GitHub and Jenkins server to detect new branches, changes in the existing branches which will trigger pipeline runs on Jenkins. There is also, a Jenkinsfile in each branch with the necessary "When" conditions to Run/skip build stages to support a Jenkins based multibranch pipeline setup.

When it comes to the deployment aspects, this Jenkins based multistage build job supports end-to-end ci/cd pipeline for the "stage" branch code deployment onto its own GKE cluster "CKE-Stage-Cluster", and another end-to-end CI/CD pipeline deployment for the "master" branch code deployment onto to its own GKE cluster " GKE-Production-cluster". Both these Kubernetes clusters are created and managed through IaC using Terraform and the associated repository URL.


# Review of the files present in this repository
 - Server.js, package.json: These are the two main files implementing the application functionality.
 - Jenkinsfile: This file has various stages & steps for the build, delivery to Docker Hub and deployment of the application. It also has the necessary logic in place to skip some of the build steps based on the Github branch.
 - Dockerfile: This file has the necessary steps to create a docker image for the application.
 - deployment.yaml: This file has the necessary code to create Kubernetes resources ( Deployment and a Servie).
 - This README.file


# Pre-requisites

1. A Jenkins server instance with necessary plugins installed & credentials configured.
2. A development server instance with Docker engine installed and configured.
3. Docker hub account and credentials to push images.
4. GitHub account and Git client.
5. GKE Cluster with necessary service accounts, roles configured to deploy the application. 

# CI/CD Pipeline workflow

1. Create a multibranch pipeline job on to the Jenkins server instance.
    - Branch source: Git, project repository: This repository URL, credentials: GitHub credentials.
    - Build configuration Mode: by Jenkinsfile, Script path: Jenkinsfile.
    - Scan multibranch pipeline Triggers: Scan by webhook.
2. Program the GitHub Webhook with the Jenkins URL ex: "JENKINS_URL/multibranch-webhook-trigger/invoke?token=[Trigger token]" to push repo/branch changes notifications payloads to Jenkins server.
3. Add the necessary branch protections to your GitHub repository. The Current implementation requires a pull request for merging branches with at least 1 approval to merge.
4. Prepare both the Stage and production GKE clusters, Docker machine, Jenkins to Docker integration.
5. To trigger a CI/CD pipeline, clone this repository onto your local, make some changes onto the files from "stage" branch and commit. Ex: You can Edit the "hello world" text from the application or can edit something on this readme file.
6. Above step will trigger a CI/CD pipeline run on the "stage" branch in Jenkins, which will pull the source code from GitHub, build a docker image, push the docker image onto Docker Hub, finally deploy the built docker image onto the GKE Stage cluster.
7. Also, on the GitHub screen you will see an option to create a pull request for merging these changes onto "master" branch. 
8. When you create that pull request, the collaborators/reviewers of this repository will be notified via email to review and approve the pull request.
9. When the pull request is approved, merged onto master, it will trigger another CI/CD pipeline, this time on the master branch.
10.This master branch pipeline also, will pull the source code from GitHub, build a Docker image, Push the image onto Docker Hub, Deploy the production image onto GKE production cluster. 

# Next Steps and possible improvements:
  - Upgrade to GitHub Organization/enterprise or GitLab account to implement private repositories, and to provide restricted access (view/read) only access to some users of this repo.
  - Implement some test cases for the application and vulnerability scanning setup through tools like SonarQube.
  - Implement a sample backend database for this Hello World application.
  - Operational support documentation covering Google Cloud Operations Suite.
  - Explore the options to configure EFK or Prometheus/Grafana configuration onto the GKE cluster.
  - Deploy the application onto other cloud providers like AWS.
