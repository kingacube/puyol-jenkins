pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('kingakube')  // Use your DockerHub credentials ID
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
                docker build -t ${DOCKER_IMAGE_TAG} -f python-web-app/Dockerfile python-web-app
                """
            }
        }

        stage('Login to DockerHub') {
            steps {
                // Log into DockerHub using the credentials
                sh """
                echo ${DOCKERHUB_CREDENTIALS_PSW} | docker login -u ${DOCKERHUB_CREDENTIALS_USR} --password-stdin
                """
            }
        }

        stage('Push Docker Image') {
            steps {
                // Push the Docker image to DockerHub
                sh """
                docker push ${DOCKER_IMAGE_TAG}
                """
            }
        }
    }

    post {
        success {
            echo 'Docker image built and pushed successfully!'
        }
        failure {
            echo 'Build or push failed!'
        }
    }
}
