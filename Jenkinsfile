pipeline {
    agent any
    environment {
        DOCKER_HUB_CREDENTIALS = credentials('docker-hub-credentials')
        TELEGRAM_BOT_TOKEN     = credentials('telegram-bot-token')
        TELEGRAM_CHAT_ID       = credentials('telegram-chat-id')
        DOCKER_IMAGE           = "keyvan1368/jenkins:${BUILD_NUMBER}"
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
            sh '''
                curl -s -X POST "https://telegram.org{TELEGRAM_BOT_TOKEN}/sendMessage" \
                    -d "chat_id=${TELEGRAM_CHAT_ID}" \
                    --data-urlencode "text=✅ Jenkins Build Succeeded!
Job: ${JOB_NAME} #${BUILD_NUMBER}
Image pushed: ${DOCKER_IMAGE}
Build URL: ${BUILD_URL}"
            '''
        }
        always {
            sh 'docker logout || true'
        }
    }

}
