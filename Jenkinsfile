pipeline {
    agent any

    tools {
        jdk 'Java17'
    }

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
            when {
                expression { false }
            }
            steps {
                echo 'SAST skipped'
            }
        }

        stage('Dependency Check') {
            when {
                expression { false }
            }
            steps {
                echo 'Dependency Check skipped'
            }
        }

        stage('Docker Build') {
            when {
                expression { false }
            }
            steps {
                echo 'Docker build skipped'
            }
        }

        stage('Trivy Scan') {
            steps {
                bat '"C:\\Users\\deffe\\AppData\\Local\\Microsoft\\WinGet\\Packages\\AquaSecurity.Trivy_Microsoft.Winget.Source_8wekyb3d8bbwe\\trivy.exe" image --format table -o trivy-report.txt juice-shop-devsecops:latest'
                bat 'type trivy-report.txt'
            }
        }

        stage('Checkov Scan') {
            steps {
                bat '"C:\\Users\\deffe\\AppData\\Local\\Programs\\Python\\Python313\\python.exe" "C:\\Users\\deffe\\AppData\\Local\\Programs\\Python\\Python313\\Scripts\\checkov" -d . --skip-path .github --soft-fail > checkov-report.txt'
                bat 'type checkov-report.txt'
            }
        }

        stage('Docker Run') {
            steps {
                bat 'docker network create juice-shop-net || exit /b 0'
                bat 'docker rm -f juice-shop-container || exit /b 0'
                bat 'docker run -d --name juice-shop-container --network juice-shop-net -p 3000:3000 juice-shop-devsecops:latest'
            }
        }
        
        stage('ZAP Scan (DAST)') {
            steps {
                bat 'docker run --rm --network juice-shop-net -t -v "%cd%:/zap/wrk/:rw" ghcr.io/zaproxy/zaproxy:stable zap-baseline.py -t http://juice-shop-container:3000 -r zap-report.html || exit /b 0'
                bat 'type zap-report.html'
            }
        }
    }
}
