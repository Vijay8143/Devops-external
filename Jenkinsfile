pipeline {
    agent any

    environment {
        DOCKER_USERNAME = 'vijayreddygoli811'
        IMAGE_NAME = 'my-app'
        DOCKER_CREDENTIALS_ID = 'dockerhub'
    }

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    echo "Building Docker image..."
                    
                    dockerImage = docker.build("${env.DOCKER_USERNAME}/${env.IMAGE_NAME}:${env.BUILD_NUMBER}")
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    echo "Pushing Docker image to Docker Hub..."
                    
                    docker.withRegistry('https://index.docker.io/v1/', env.DOCKER_CREDENTIALS_ID) {
                        dockerImage.push()
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}