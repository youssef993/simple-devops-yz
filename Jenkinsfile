pipeline {
    agent any

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
                docker build -t youssefz93/simple-devops-yz .
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
                docker push youssefz93/simple-devops-yz
                """
            }
        }
    }
}
