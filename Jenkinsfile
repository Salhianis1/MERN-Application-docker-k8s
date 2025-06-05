pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('docker-hub-credentials') // Jenkins credentials ID
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

        stage('Docker Login') {
                    steps {
                        withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials',
                                                         usernameVariable: 'DOCKERHUB_USER',
                                                         passwordVariable: 'DOCKERHUB_PASS')]) {
                            sh "echo $DOCKERHUB_PASS | docker login -u $DOCKERHUB_USER --password-stdin"
                        }
                    }
                }
        
                stage('Push to Docker Hub') {
                    steps {
                        script {
                            sh "docker push $IMAGE_NAME:$IMAGE_TAG"
                        }
                    }
                }
    }

    post {
        success {
            echo '✅ Build and push completed successfully!'
        }
        failure {
            echo '❌ Build or push failed.'
        }
    }
}
