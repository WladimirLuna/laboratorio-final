pipeline {
    agent any

    environment {
        SONARQUBE_URL = 'https://sonarcloud.io'
        SONAR_PROJECT_KEY = 'wladimirluna_laboratorio-final'
        SONAR_ORGANIZATION = 'wladimirluna'
        SONAR_TOKEN = credentials('SONAR_TOKEN')
        DOCKER_IMAGE = 'laboratorio-final-app'
        DOCKER_TAG = 'latest'
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

        // stage('Run Tests and Generate Coverage') {
        //     steps {
        //         script {
        //             bat 'docker run --rm -v "%cd%":/app -w /app node:14 npm run test'
        //         }
        //     }
        // }

        //  stage('SonarQube Analysis') {
        //     steps {
        //         script {
        //             bat '''
        //             docker run --rm -v "%cd%":/app -w /app sonarsource/sonar-scanner-cli ^
        //                 -Dsonar.projectKey=%SONAR_PROJECT_KEY% ^
        //                 -Dsonar.organization=%SONAR_ORGANIZATION% ^
        //                 -Dsonar.host.url=%SONARQUBE_URL% ^
        //                 -Dsonar.login=%SONAR_TOKEN% ^
        //                 -Dsonar.exclusions=node_modules/** ^
        //                 -Dsonar.javascript.lcov.reportPaths=coverage/lcov.info
        //             '''
        //         }
        //     }
        // }

        stage('Construir Imagen Docker') {
            steps {
                bat 'docker build -t %DOCKER_IMAGE%:%DOCKER_TAG% .'
            }
        }

        // stage('Analizar Imagen Docker') {
        //     steps {
        //         bat 'C:/Users/InTheMix/Documents/Diplomado/DevSecOps/trivy/trivy.exe image --scanners vuln laboratorio-final-app:latest'
        //     }
        // }
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