pipeline {
    agent any 

    environment {
        DOCKER_IMAGE = 'cithit/alhayen'  // Replace 'alhayen' with your Miami ID if different
        IMAGE_TAG = "build-${BUILD_NUMBER}"
        GITHUB_URL = 'https://github.com/Neshmi9/lab3-4.git'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']],
                          userRemoteConfigs: [[url: "${GITHUB_URL}"]]])
            }
        }

        stage('Lint HTML') {
            steps {
                sh 'npm install htmlhint --save-dev'
                sh 'npx htmlhint *.html'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE}:${IMAGE_TAG}")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                echo 'Skipping Docker push (no credentials configured)'
            }
        }

        stage('Deploy to Dev Environment') {
            steps {
                echo 'Skipping Dev deployment (Kubernetes disabled for this lab)'
            }
        }

        stage('Deploy to Prod Environment') {
            steps {
                echo 'Skipping Prod deployment (Kubernetes disabled for this lab)'
            }
        }

        stage('Check Kubernetes Cluster') {
            steps {
                echo 'Skipping Kubernetes checks (not required)'
            }
        }
    }

    post {
        success {
            slackSend (
                webhookUrl: credentials('slack-webhook'),
                message: "✅ Build Succeeded: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                color: '#36a64f'
            )
        }
        failure {
            slackSend (
                webhookUrl: credentials('slack-webhook'),
                message: "❌ Build Failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                color: '#ff0000'
            )
        }
    }
}
