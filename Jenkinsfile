pipeline {
    agent any
    environment {
        PROJECT_ID = 'olivealex'
        CLUSTER_NAME = 'test-cluster'
        LOCATION = 'europe-west2-c'
        CREDENTIALS_ID = 'olivealex'
    }
    stages {
        stage("Checkout code") {
            steps {
                checkout scm
            }
        }
        stage("Build image") {
            agent {
                    docker {
                        label 'dockerserver'  // both label and image
                        image 'node:7-alpine' 
                    }
            }
            steps {
                script {
                    myapp = docker.build("gcr.io/olivealex/express-web:latest")
                }
            }
        }
        stage("Push image") {
            agent {
                    docker {
                        label 'dockerserver'  // both label and image
                        image 'node:7-alpine' 
                    }
            }
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                            myapp.push("latest")
                    }
                }
            }
        }        
        stage('Deploy to GKE') {
            steps{
                sh "sed -i 's/express-web:latest/express-web:latest/g' deployment.yaml"
                step([$class: 'KubernetesEngineBuilder', projectId: env.PROJECT_ID, clusterName: env.CLUSTER_NAME, location: env.LOCATION, manifestPattern: 'deployment.yaml', credentialsId: env.CREDENTIALS_ID, verifyDeployments: true])
            }
        }
    }    
}