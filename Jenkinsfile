pipeline {
    agent any

    environment {
        IMAGE_NAME = 'kirmada'
        CONTAINER_NAME = 'kirmanda'
        PORT = '80'
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/demonikkk/breathefree.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${IMAGE_NAME} ."
                }
            }
        }

        stage('Deploy Container') {
            steps {
                script {
                    sh """
                    docker kill ${CONTAINER_NAME} | true
                    docker rm ${CONTAINER_NAME} | true
                    docker run -d -p 8000:${PORT} --name ${CONTAINER_NAME} ${IMAGE_NAME}
                    """
                }
            }
        }
    }

    post {
        success {
            echo "Site deployed successfully"
        }
        failure {
            echo "Deployment failed. Please check the logs."
        }
    }
}
