pipeline {
    agent any
    environment {
        PROJECT_ID = 'bitnami--yv-rtlewq'
        STAGE_CLUSTER_NAME = 'cicdpipelinestage'
        PROD_CLUSTER_NAME = 'cicdpipelineproduction'
        LOCATION = 'us-east1'
        CREDENTIALS_ID = 'cicdpipeline'
    }
    stages {
        stage("Checkout code") {
            steps {
                checkout scm
            }
        }
        stage("Build image") {
            steps {
                script {
                    myapp = docker.build("nkarumanchi27/hello:${env.BUILD_ID}")
                }
            }
        }
        stage("Push image") {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhubcred') {
                            myapp.push("latest")
                            myapp.push("${env.BUILD_ID}")
                    }
                }
            }
        }
        stage('Deploy to GKE stage environment') {
             when {

          	branch "stage"

             }
            	steps{
                    sh "sed -i 's/hello:latest/hello:${env.BUILD_ID}/g' deployment.yaml"
                    step([$class: 'KubernetesEngineBuilder', projectId: env.PROJECT_ID, clusterName: env.STAGE_CLUSTER_NAME, location: env.LOCATION, manifestPattern: 'deployment.yaml', credentialsId: env.CREDENTIALS_ID, verifyDeployments: true])
            }
        }
        stage('Deploy to GKE Production environment') {
             when {

          	branch "master"

             }
            	steps{
                    sh "sed -i 's/hello:latest/hello:${env.BUILD_ID}/g' deployment.yaml"
                    step([$class: 'KubernetesEngineBuilder', projectId: env.PROJECT_ID, clusterName: env.PROD_CLUSTER_NAME, location: env.LOCATION, manifestPattern: 'deployment.yaml', credentialsId: env.CREDENTIALS_ID, verifyDeployments: true])
            }
        }

    }
}
