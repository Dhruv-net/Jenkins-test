pipeline {
    agent any

    environment {
        DOCKER_IMAGE_NAME = "python-app"
        DOCKER_CONTAINER_NAME = "python-app-container"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Build') {
            steps {
                sh 'pip install -r requirements.txt'
            }
        }
        
        stage('Test') {
            steps {
                sh 'python -m pytest'
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE_NAME}:${env.BUILD_NUMBER}")
                }
            }
        }
        
        stage('Run Docker Container') {
            steps {
                script {
                    sh """
                        docker stop ${DOCKER_CONTAINER_NAME} || true
                        docker rm ${DOCKER_CONTAINER_NAME} || true
                        docker run -d --name ${DOCKER_CONTAINER_NAME} -p 5000:5000 ${DOCKER_IMAGE_NAME}:${env.BUILD_NUMBER}
                    """
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}