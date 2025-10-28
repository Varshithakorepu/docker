pipeline {
    agent any

    environment {
        IMAGE_NAME = 'flask-jenkins'
        DOCKER_HUB_REPO = 'varshitha1105/flask-jenkins'
    }

    stages {
        stage('Checkout Code') {
            steps {
                echo '📦 Checking out source code...'
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                echo '🏗️ Building Docker image...'
                bat "docker build -t %IMAGE_NAME% ."
            }
        }

        stage('Tag Docker Image') {
            steps {
                echo '🏷️ Tagging Docker image for Docker Hub...'
                bat "docker tag %IMAGE_NAME% %DOCKER_HUB_REPO%:latest"
            }
        }

        stage('Login to Docker Hub') {
            steps {
                echo '🔐 Logging in to Docker Hub...'
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'DOCKERHUB_USER', passwordVariable: 'DOCKERHUB_PASS')]) {
                    bat """
                        echo %DOCKERHUB_PASS% | docker login -u %DOCKERHUB_USER% --password-stdin
                    """
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                echo '🚀 Pushing Docker image to Docker Hub...'
                bat "docker push %DOCKER_HUB_REPO%:latest"
            }
        }
    }

    post {
        success {
            echo '✅ Docker image built, tagged, and pushed successfully to Docker Hub!'
        }
        failure {
            echo '❌ Build failed! Please check the Jenkins logs for details.'
        }
    }
}
