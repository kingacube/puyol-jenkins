pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub') // Docker Hub credentials
        DOCKERHUB_REPO = 'kingakube/python-web' // Your Docker Hub repo
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout code from the version control repository
                checkout scm
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    // Corrected Docker build command
                    // Make sure the path to Dockerfile is correct
                    sh '''
                    docker build -t ${DOCKERHUB_REPO}:${env.BUILD_NUMBER} -f python-web-app/Dockerfile python-web-app
                    '''
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    // Log in to Docker Hub and push the Docker image
                    docker.withRegistry('https://registry.hub.docker.com', "${env.DOCKERHUB_CREDENTIALS}") {
                        def image = docker.image("${DOCKERHUB_REPO}:${env.BUILD_NUMBER}")
                        image.push()
                    }
                }
            }
        }
    }
}
