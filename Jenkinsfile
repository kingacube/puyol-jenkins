pipeline {
    agent any

    environment {
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

        stage('Check requirements.txt') {
            steps {
                // Ensure that requirements.txt is present in the python-web-app directory
                script {
                    def requirementsExists = fileExists 'python-web-app/requirements.txt'
                    if (!requirementsExists) {
                        error "requirements.txt file not found in python-web-app directory. Please add it before proceeding."
                    }
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                // Build Docker image using the provided Dockerfile
                sh """
                docker build -t ${DOCKER_IMAGE_TAG} -f python-web-app/Dockerfile python-web-app
                """
            }
        }

        stage('Login to DockerHub') {
            steps {
                // Securely log into DockerHub using credentials
                withCredentials([usernamePassword(credentialsId: 'kingakube', usernameVariable: 'DOCKERHUB_USERNAME', passwordVariable: 'DOCKERHUB_PASSWORD')]) {
                    sh """
                    echo "${DOCKERHUB_PASSWORD}" | docker login -u "${DOCKERHUB_USERNAME}" --password-stdin
                    """
                }
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
