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

        stage('Check Docker') {
            steps {
                sh 'docker info'
            }
        }

        stage('Check Python') {
            steps {
                sh 'python3 --version'
                sh 'pip3 --version'
            }
        }

        stage('Check Dockerfile') {
            steps {
                sh 'ls -l Dockerfile.python'
            }
        }

        stage('Check Port') {
            steps {
                sh 'netstat -tuln | grep :5000 || echo "Port 5000 is free"'
            }
        }

        stage('Build') {
            steps {
                sh 'pip3 install -r requirements.txt'
            }
        }

        stage('Test') {
            steps {
                sh 'python3 -m pytest'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE_NAME}:${env.BUILD_NUMBER}", "-f Dockerfile.python .")
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