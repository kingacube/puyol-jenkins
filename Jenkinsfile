pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-credentials') // Docker Hub credentials stored in Jenkins
        DOCKERHUB_REPO = 'kingakube/python-web' // Updated with your Docker Hub username and image name
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
                    // Build Docker image using the Dockerfile
                    def image = docker.build("${env.DOCKERHUB_REPO}:${env.BUILD_NUMBER}")
                }
            }
        }
