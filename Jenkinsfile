pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'cithit/alhayen'
        IMAGE_TAG = "build-${BUILD_NUMBER}"
        GITHUB_URL = 'https://github.com/Neshmi9/lab3-4.git'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM',
                          branches: [[name: '*/main']],
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
                echo 'Skipping Docker push (not configured)'
            }
        }

        stage('Deploy to Dev Environment') {
            steps {
                echo 'Skipping Dev deploy (not required)'
            }
        }

        stage('Deploy to Prod Environment') {
            steps {
                echo 'Skipping Prod deploy (not required)'
            }
        }

        stage('Check Kubernetes Cluster') {
            steps {
                echo 'Skipping Kubernetes check'
            }
        }
    }

    post {
        success {
            slackSend (
                channel: '#builds',
                color: 'good',
                message: "Build Succeeded: ${env.JOB_NAME} #${env.BUILD_NUMBER}"
            )
        }
        failure {
            slackSend (
                channel: '#builds',
                color: 'danger',
                message: "Build Failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}"
            )
        }
    }
}
