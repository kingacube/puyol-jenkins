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
                            sh """
                            docker build -t ${DOCKERHUB_REPO}:${env.BUILD_NUMBER} -f python-web-app/Dockerfile python-web-app
                            """
                        }
                    }
                }

                stage('Push Docker Image') {
                    steps {
                        script {
                            docker.withRegistry('https://registry.hub.docker.com', "${env.DOCKERHUB_CREDENTIALS}") {
                                def image = docker.image("${DOCKERHUB_REPO}:${env.BUILD_NUMBER}")
                                image.push()
                            }
                        }
                    }
                }
            }
        }
    }
}
