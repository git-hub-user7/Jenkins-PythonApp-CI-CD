pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-creds')
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', 
                url: 'https://github.com/git-hub-user7/Jenkins-PythonApp-CI-CD.git'
            }
        }
        stage('Test') {
            steps {
                bat 'pytest app/test_app.py'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("dockerprovider/python-app:${env.BUILD_ID}")
                }
            }
        }
        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub-creds') {
                        dockerImage.push()
                    }
                }
            }
        }
    }
}
