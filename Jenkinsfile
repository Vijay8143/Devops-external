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
                    echo "Building Docker image: ${env.DOCKER_USERNAME}/${env.IMAGE_NAME}:${env.BUILD_NUMBER}"
                    docker.build("${env.DOCKER_USERNAME}/${env.IMAGE_NAME}:${env.BUILD_NUMBER}", '.')
                }
            }
        }
        
        stage('Push to Docker Hub') {
            steps {
                script {
                    echo "Pushing Docker image to Docker Hub..."
                    docker.withRegistry('https://index.docker.io/v1/', env.DOCKER_CREDENTIALS_ID) {
                        
                        docker.image("${env.DOCKER_USERNAME}/${env.IMAGE_NAME}:${env.BUILD_NUMBER}").push()

                        
                        docker.image("${env.DOCKER_USERNAME}/${env.IMAGE_NAME}:${env.BUILD_NUMBER}").push('latest')
                    }
                }
            }
        }
    }

    post {
        always {
            // Clean up the local Docker images on the Jenkins agent to save space
            echo "Cleaning up local Docker images..."
            sh "docker rmi ${env.DOCKER_USERNAME}/${env.IMAGE_NAME}:${env.BUILD_NUMBER}"
            sh "docker rmi ${env.DOCKER_USERNAME}/${env.IMAGE_NAME}:latest"
        }
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}