pipeline {
    agent any

    environment {
        VM_IP = "192.168.56.11"           // IP de ta VM VirtualBox
        VM_USER = "vagrant"               // utilisateur de la VM
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
                docker build -t %DOCKER_IMAGE% .
                """
            }
        }
        
        stage('Login to Docker Hub') {
            steps {
                withCredentials([string(credentialsId: 'DOCKER_PASS', variable: 'DOCKER_PWD')]) {
                    bat """
                    echo %DOCKER_PWD% | docker login -u youssefz93 --password-stdin
                    """
                }
            }
        }
        
        stage('Push Docker Image') {
            steps {
                bat """
                docker push %DOCKER_IMAGE%
                """
            }
        }

       steps {
        bat """
        ssh -i C:\Users\youss\Desktop\ansible-formation\.vagrant\machines\node-1\virtualbox\private_key -o StrictHostKeyChecking=no vagrant@192.168.56.11 ^
        "kubectl set image deployment/simple-devops-yz simple-devops-yz=youssefz93/simple-devops-yz:latest --record && kubectl rollout status deployment/simple-devops-yz"
        """
    }
    }
}
