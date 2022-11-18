# CI/CD Pipeline repository for a Dockerized Hello World application
This repository have everything you needed to build and deploy a sample Hello World application on to the Google Kubernetes Engine. 
Application files: Server.js, package.json are the two files implementing the application functionality.


# Pre-requisites

1. A Jenkins server instance with necessary plugins installed.
2. A devlopment server instance with Docker engine installed and configured.
3. GKE Cluster with necesary service accounts and configurations to deploy the application. 

# CI/CD Pipeline workflow

1. Create a multibranch pipeline on to your jenkins server instance.  
