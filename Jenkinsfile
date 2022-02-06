pipeline {
    agent any
    
    environment {
    dockerimagename = "rstraining4/nodeapp"
    dockerImage = ""
    registryCredential = 'dockerhublogin'
    }

    stages {
        stage('Git Checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/rstraining4/k8s-deployment.git']]])
            }
        }
        stage('Build Docker Image') {
            steps{
                script {
                    dockerImage = docker.build dockerimagename
                }
            }
        }

        //stage('Build image') {
        //    steps{
        //        script {
        //            dockerImage = docker.build dockerimagename
        //        }
        //    }
        //}

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
                        dockerImage.push("latest")
                    }
                }
            }
        }
        stage('Deploying App to Kubernetes') {
            steps {
                script {
                    kubernetesDeploy(configs: "deploymentservice.yaml", kubeconfigId: "kubernetes")
                }
            }
        }
    }
}
