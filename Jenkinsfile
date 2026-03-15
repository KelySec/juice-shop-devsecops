pipeline {
    agent any

    stages {

        stage('Checkout Code') {
            steps {
                echo 'Project ready'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t juice-shop .'
            }
        }

        stage('Run Container') {
            steps {
                bat 'docker run -d -p 3001:3000 juice-shop'
            }
        }

    }
}
