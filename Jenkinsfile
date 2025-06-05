pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('docker-hub-credentials') // Jenkins credentials ID
        DOCKERHUB_USER = 'salhianis20'
        IMAGE_TAG = 'latest'
    }

        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Salhianis1/MERN-Application-docker-k8s.git'
            }
        }

        stage('Build Server Image') {
            steps {
                script {
                    docker.build("${DOCKERHUB_USER}/server:${IMAGE_TAG}", './server')
                }
            }
        }

        stage('Build Client Image') {
            steps {
                script {
                    docker.build("${DOCKERHUB_USER}/client:${IMAGE_TAG}", './client')
                }
            }
        }

        stage('Push Images to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', DOCKERHUB_CREDENTIALS) {
                        docker.image("${DOCKERHUB_USER}/server:${IMAGE_TAG}").push()
                        docker.image("${DOCKERHUB_USER}/client:${IMAGE_TAG}").push()
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Build and push completed successfully!'
        }
        failure {
            echo 'Build or push failed.'
        }
    }
}
