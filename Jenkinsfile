pipeline {
    agent any

    environment {
        APP_NAME = "snipeit"
        IMAGE_NAME = "snipeit-app"
        GIT_REPO = "https://github.com/babu120987/CICD_snipeit.git"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main', url: "${GIT_REPO}"
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                docker build -t $IMAGE_NAME:latest .
                '''
            }
        }

        stage('Test Image') {
            steps {
                sh '''
                docker run --rm $IMAGE_NAME:latest php -v
                '''
            }
        }

        stage('Deploy Containers') {
            steps {
                sh '''
                docker-compose down
                docker-compose up -d
                '''
            }
        }

        stage('Verify Running Containers') {
            steps {
                sh '''
                docker ps
                '''
            }
        }
    }

    post {
        success {
            echo "CI/CD Pipeline completed successfully 🚀"
        }
        failure {
            echo "Pipeline failed ❌ — Fix your config"
        }
    }
}
