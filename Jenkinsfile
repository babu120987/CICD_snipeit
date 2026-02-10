pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "snipeit_app:latest"
        CONTAINER_NAME = "snipeit_container"
        GIT_REPO = "https://github.com/babu120987/CICD_snipeit.git"
    }

    stages {
        stage('Checkout') {
            steps {
                git url: "${GIT_REPO}", branch: 'main'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE}")
                }
            }
        }

        stage('Maven Test') {
            steps {
                sh 'mvn clean test'
            }
        }

        stage('Run Container') {
            steps {
                script {
                    // Stop and remove existing container if running
                    sh """
                    docker stop ${CONTAINER_NAME} || true
                    docker rm ${CONTAINER_NAME} || true
                    """

                    // Run container
                    sh """
                    docker run -d --name ${CONTAINER_NAME} -p 8080:80 ${DOCKER_IMAGE}
                    """
                }
            }
        }
    }

    post {
        always {
            echo 'Cleaning up workspace...'
            cleanWs()
        }
    }
}

