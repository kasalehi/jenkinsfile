pipeline {
    agent any
    environment {
        DOCKER_HUB_CREDENTIALS = credentials('docker-hub-credentials')
        DOCKER_IMAGE = "keyvan1368/jenkins:${BUILD_NUMBER}"
    }
    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out code...'
                checkout scm
            }
        }
        stage('Build') {
            steps {
                echo 'Building...'
                bat 'docker build -t %DOCKER_IMAGE% .'
            }
        }
        stage('Login to Docker Hub') {
            steps {
                echo 'Logging into Docker Hub...'
                bat 'echo %DOCKER_HUB_CREDENTIALS_PSW%| docker login -u %DOCKER_HUB_CREDENTIALS_USR% --password-stdin'
            }
        }
        stage('Push to Docker Hub') {
            steps {
                echo 'Pushing to Docker Hub...'
                bat 'docker push %DOCKER_IMAGE%'
            }
        }
    }
    post {
        always {
            bat 'docker logout'
        }
    }
}
