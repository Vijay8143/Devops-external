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
                    
                    docker.withRegistry('https://registry.hub.docker.com', DOCKER_CREDENTIALS_ID) {
                        docker.image("${env.DOCKER_USERNAME}/${env.IMAGE_NAME}:${env.BUILD_NUMBER}").push()
                    }
                }
            }
        }
    }


    post {
// ...existing code...
    post {
       
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
}