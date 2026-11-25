groovy

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
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }
        stage('Login to Docker Hub') {
            steps {
                echo 'Logging into Docker Hub...'
                sh 'echo "$DOCKER_HUB_CREDENTIALS_PSW" | docker login -u "$DOCKER_HUB_CREDENTIALS_USR" --password-stdin'
            }
        }
        stage('Push to Docker Hub') {
            steps {
                echo 'Pushing to Docker Hub...'
                sh 'docker push $DOCKER_IMAGE'
            }
        }
    }
    post {
        success {
            mail to: 'ksalehi56@yahoo.com',
                 subject: "Success: ${currentBuild.fullDisplayName}",
                 body: "The pipeline succeeded! Image pushed: ${DOCKER_IMAGE}\n\nBuild URL: ${BUILD_URL}"
        }
        always {
            sh 'docker logout || true'
        }
    }
}

Use code with caution.

Copy code text to clipboard.
