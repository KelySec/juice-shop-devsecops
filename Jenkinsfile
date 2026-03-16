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
                script {
                    def scannerHome = tool 'SonarQubeScanner'
                    def javaHome = tool 'Java17'
                    withSonarQubeEnv('SonarQube') {
                        bat """
                        set JAVA_HOME=${javaHome}
                        set PATH=%JAVA_HOME%\\bin;%PATH%
                        ${scannerHome}\\bin\\sonar-scanner.bat -Dsonar.projectKey=juice-shop-devsecops -Dsonar.sources=.
                        """
                    }
                }
            }
        }

        stage('Dependency Check') {
            when {
                expression { false }
            }
            steps {
                withCredentials([string(credentialsId: 'nvd-api-key', variable: 'NVD_API_KEY')]) {
                    dependencyCheck additionalArguments: "--nvdApiKey ${NVD_API_KEY} --scan . --format XML --format HTML", odcInstallation: 'DependencyCheck'
                }
                dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
            }
        }

        stage('Docker Build') {
            when {
                expression { false }
            }
            steps {
                bat 'docker build -t juice-shop-devsecops .'
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
                bat 'docker rm -f juice-shop-container || exit /b 0'
                bat 'docker run -d --name juice-shop-container -p 3000:3000 juice-shop-devsecops:latest'
            }
        }

        stage('ZAP Scan (DAST)') {
            steps {
                echo 'ZAP DAST stage to be configured'
            }
        }
    }
}
