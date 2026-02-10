pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/babu120987/CICD_snipeit.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                docker build -t snipeit-app:latest .
                '''
            }
        }

        stage('Test Image') {
            steps {
                sh '''
                docker run --rm snipeit-app:latest php -v
                '''
            }
        }

        stage('Deploy Containers') {
            steps {
                sh '''
                docker rm -f snipeit-app snipeit-mysql snipeit-redis || true
                docker-compose up -d
                '''
            }
        }
    }

    post {
        always {
            cleanWs()
        }
        success {
            echo "✅ Snipe-IT deployed successfully on port 8088"
        }
        failure {
            echo "❌ Pipeline failed — check logs"
        }
    }
}
