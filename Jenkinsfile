pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub') // Updated Docker Hub credentials token
        DOCKERHUB_REPO = 'kingakube/python-web' // Your Docker Hub username and image name
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
                    sh'''
                    Docker build -t kingacube/python-web -f python-web-app/Dockerfile .
                    '''
                   
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    // Log in to Docker Hub
                    docker.withRegistry('https://registry.hub.docker.com', "${env.DOCKERHUB_CREDENTIALS}") {
                        // Push the Docker image to Docker Hub
                        def image = docker.build("${env.DOCKERHUB_REPO}:${env.BUILD_NUMBER}", "python-web-app")
                        image.push()
                    }
                }
            }
        }
    }
}
