pipeline {
    agent any

    environment {
        VM_IP = "192.168.56.11"
        VM_USER = "vagrant"
        DOCKER_IMAGE = "youssefz93/simple-devops-yz:latest"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/youssef993/simple-devops-yz'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat """
                docker build -t ${env.DOCKER_IMAGE} .
                """
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([string(credentialsId: 'DOCKER_PASS', variable: 'DOCKER_PWD')]) {
                    bat """
                    echo ${env.DOCKER_PWD} | docker login -u youssefz93 --password-stdin
                    """
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                bat """
                docker push ${env.DOCKER_IMAGE}
                """
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                bat """
                ssh -i C:/Users/youss/Desktop/ansible-formation/.vagrant/machines/node-1/virtualbox/private_key -o StrictHostKeyChecking=no vagrant@${env.VM_IP} ^
                "kubectl set image deployment/simple-devops-yz simple-devops-yz=${env.DOCKER_IMAGE} --record && kubectl rollout status deployment/simple-devops-yz"
                """
            }
        }
    }
}
