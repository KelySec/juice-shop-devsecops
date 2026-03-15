pipeline {
    agent any

    stages {

        stage('Checkout SCM') {
            steps {
                echo 'Source code retrieved from GitHub by Jenkins SCM'
            }
        }

        stage('Build Application') {
            steps {
                bat 'npm install'
            }
        }

        stage('SAST Scan') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    bat 'sonar-scanner.bat -Dsonar.projectKey=juice-shop-devsecops -Dsonar.sources=.'
                }
            }
        }

        stage('Dependency Check') {
            steps {
                echo 'Dependency check stage to be configured'
            }
        }

        stage('Docker Build') {
            steps {
                echo 'Docker build stage to be configured'
            }
        }

        stage('Trivy Scan') {
            steps {
                echo 'Trivy scan stage to be configured'
            }
        }

        stage('Checkov Scan') {
            steps {
                echo 'Checkov scan stage to be configured'
            }
        }

        stage('Docker Run') {
            steps {
                echo 'Docker run stage to be configured'
            }
        }

        stage('ZAP Scan (DAST)') {
            steps {
                echo 'ZAP DAST stage to be configured'
            }
        }
    }
}
