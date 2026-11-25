

pipeline {
    agent any
    environment {
        DOCKER_hub_CREDENTIALS = credentials('docker-hub-credentials')
        DoCKER_IMAGE = 'keyvan1368/jenkins:$BUILD_NUMBER'
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
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }
        stage('logindockerhub') {
            steps {
                echo 'Logging into Docker Hub...'
                sh 'docker login -u $DOCKER_hub_CREDENTIALS_USR -p $DOCKER_hub_CREDENTIALS_PSW'
            }
        }
        stage('Pushdockerhub') {
            steps {
                echo 'Pushing to Docker Hub...'
                sh 'docker push $DOCKER_IMAGE'
            }
        }
    }
}
