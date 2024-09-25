pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub')
        DOCKERHUB_REPO = 'kingakube/python-web'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Build and Push') {
            parallel {
                stage('Build Docker Image') {
                    steps {
                        script {
                            def imageTag = "${DOCKERHUB_REPO}:${env.BUILD_NUMBER}"
                            try {
                                sh """
                                docker build -t ${imageTag} -f python-web-app/Dockerfile python-web-app
                                """
                            } catch (Exception e) {
                                error("Docker build failed: ${e.message}")
                            }
                        }
                    }
                }

                stage('Push Docker Image') {
                    steps {
                        script {
                            def imageTag = "${DOCKERHUB_REPO}:${env.BUILD_NUMBER}"
                            docker.withRegistry('https://registry.hub.docker.com', "${DOCKERHUB_CREDENTIALS}") {
                                def image = docker.image(imageTag)
                                try {
                                    image.push()
                                } catch (Exception e) {
                                    error("Docker push failed: ${e.message}")
                                }
                            }
                        }
                    }
                }
            }
        }
    }

    post {
        success {
            echo "Build and push succeeded!"
        }
        failure {
            echo "Build or push failed!"
        }
    }
}
