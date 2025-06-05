pipeline {
    agent any

    environment {
        DOCKERHUB_USER = 'salhianis20'
        IMAGE_TAG = 'latest'
    }

    stages {
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

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'DOCKERHUB_USER_CRED', passwordVariable: 'DOCKERHUB_PASS')]) {
                    sh """
                        echo "$DOCKERHUB_PASS" | docker login -u "$DOCKERHUB_USER_CRED" --password-stdin
                    """
                }
            }
        }

        stage('Push Images to Docker Hub') {
            steps {
                script {
                    docker.image("${DOCKERHUB_USER}/server:${IMAGE_TAG}").push()
                    docker.image("${DOCKERHUB_USER}/client:${IMAGE_TAG}").push()
                }
            }
        }
    }

    post {
        failure {
            echo '❌ Build or push failed.'
        }
        success {
            echo '✅ Build and push completed successfully.'
        }
    }
}
