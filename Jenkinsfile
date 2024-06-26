pipeline {
    agent any

    environment {
        DOCKER_CREDENTIALS_ID = 'docker-hub-credentials'
        DOCKER_IMAGE = 'gvasov/test_project'
    }

    stages {
        stage('Initialize'){
            steps {
                script {
                    def dockerHome = tool 'myDocker'
                    env.PATH = "${dockerHome}/bin:${env.PATH}"
                }
            }
        }
                      

        stage('Checkout') {
            steps {
                git 'https://github.com/gogisty/DevOpsTest.git'
            }
        }    

        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image
                    docker.build(DOCKER_IMAGE)
                }
            }
        }

        stage('Push Docker Image to Docker Hub') {
            steps {
                script {
                    // Log in to Docker Hub
                    docker.withRegistry('https://registry.hub.docker.com', DOCKER_CREDENTIALS_ID) {
                        // Push the Docker image to Docker Hub
                        docker.image("${DOCKER_IMAGE}").push()
                    }
                }
            }
        }
    }
        post {
        success {
            echo 'Build and Push to Docker Hub successful!'
        }
        failure {
            echo 'Build or Push to Docker Hub failed!'
        }
    }
}