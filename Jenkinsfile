pipeline {
    agent any

    environment {
        IMAGE_NAME = "flask-app:latest"
        KIND_CLUSTER = "flask-cluster"
        K8S_YAML = "k8s-deployment.yaml"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/anilkumarmidde09/flask-docker-k8s-jenkins.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    echo "Building Docker image: ${IMAGE_NAME}"
                    sh 'docker --version || echo "Docker not installed!"'
                    sh "docker build -t ${IMAGE_NAME} ."
                }
            }
        }

        stage('Load Image into KIND') {
            steps {
                script {
                    sh 'which kind || echo "KIND is not installed or not in PATH!"'
                    sh "kind load docker-image ${IMAGE_NAME} --name ${KIND_CLUSTER}"
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    sh 'which kubectl || echo "kubectl not found!"'
                    sh "kubectl apply -f ${K8S_YAML}"
                }
            }
        }
    }
}
