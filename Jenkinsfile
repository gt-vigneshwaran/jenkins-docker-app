pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Image') {
            steps {
                bat 'docker build -t jenkins-app:build-%BUILD_NUMBER% .'
            }
        }

        stage('Deploy & Verify') {
            steps {
                bat '''
                docker stop jenkins-app 2>nul || exit /b 0
                docker rm jenkins-app 2>nul || exit /b 0
                docker run -d --name jenkins-app -p 8081:80 jenkins-app:build-%BUILD_NUMBER%
                '''
                bat 'curl http://localhost:8081'
            }
        }
    }
}