pipeline {
    agent any

    environment {
        SONARQUBE_URL = 'https://sonarcloud.io'
        SONAR_PROJECT_KEY = 'laboratorio-final'
        SONAR_ORGANIZATION = 'WladimirLuna'
    }

    stages {
        stage('Checkout') {
            steps {
                // Descargamos el código del repositorio
                git url: 'https://github.com/WladimirLuna/laboratorio-final', branch: 'feature-lab-final'
            }
        }
        stage('Install Dependencies') {
            steps {
                script {
                    bat 'docker run --rm -v "%cd%":/app -w /app node:14 npm install'
                }
            }
        }
        stage('Build') {
            steps {
                script {
                    bat 'docker run --rm -v "%cd%":/app -w /app node:14 npm run build'
                }
            }
        }

        stage('Run Tests and Generate Coverage') {
            steps {
                script {
                    bat 'docker run --rm -v "%cd%":/app -w /app node:14 npm run test'
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully'
        }
        failure {
            echo 'Pipeline failed due to failed quality gate condition'
        }
    }
}