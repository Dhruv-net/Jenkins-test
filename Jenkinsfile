pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Run Python Script') {
            steps {
                sh 'python3 app.py'
            }
        }
    }
}