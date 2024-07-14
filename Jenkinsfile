pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Build') {
            steps {
                sh 'your-build-command-here'
            }
        }
        
        stage('Test') {
            steps {
                sh 'your-test-command-here'
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("your-docker-image-name:${env.BUILD_NUMBER}")
                }
            }
        }
        
        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://your-docker-registry-url', 'docker-credentials-id') {
                        docker.image("your-docker-image-name:${env.BUILD_NUMBER}").push()
                    }
                }
            }
        }
        
        stage('Deploy to EC2') {
            steps {
                sshagent(credentials: ['your-ssh-credentials-id']) {
                    sh '''
                        ssh user@your-ec2-instance-ip "
                            docker pull your-docker-registry-url/your-docker-image-name:${BUILD_NUMBER}
                            docker stop your-container-name || true
                            docker rm your-container-name || true
                            docker run -d --name your-container-name -p 80:80 your-docker-registry-url/your-docker-image-name:${BUILD_NUMBER}
                        "
                    '''
                }
            }
        }
    }
}