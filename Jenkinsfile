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
                    
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                        bat "echo ${env.DOCKER_PASS} | docker login -u ${env.DOCKER_USER} --password-stdin"
                        bat "docker push vijayreddygoli811/my-app:${env.BUILD_NUMBER}"
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