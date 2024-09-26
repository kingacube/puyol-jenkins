pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('kingkube') // DockerHub credentials from Jenkins
        DOCKERHUB_REPO = 'kingakube/python-web'           // Your DockerHub repository
        DOCKER_IMAGE_TAG = "${DOCKERHUB_REPO}:${env.BUILD_NUMBER}" // Tag image with build number
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout source code from version control
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                // Build Docker image
                sh """
                docker build -t ${DOCKER_IMAGE_TAG} -f python-web-app/Dockerfile .
                """
            }
        }

    }

    post {
        success {
            echo 'Docker image built successfully!'
        }
        failure {
            echo 'Build failed!'
        }
    }
}
