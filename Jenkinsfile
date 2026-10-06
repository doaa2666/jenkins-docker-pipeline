pipeline {
    agent any

    environment {
        IMAGE_NAME = 'doaa2026/jenkins-docker-demo'
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Source code was checked out by Jenkins'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:$BUILD_NUMBER .'
                sh 'docker tag $IMAGE_NAME:$BUILD_NUMBER $IMAGE_NAME:latest'
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-creds',
                        usernameVariable: 'DOCKER_USERNAME',
                        passwordVariable: 'DOCKER_PASSWORD'
                    )
                ]) {
                    sh 'echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin'
                    sh 'docker push $IMAGE_NAME:$BUILD_NUMBER'
                    sh 'docker push $IMAGE_NAME:latest'
                }
            }
        }
    }

    post {
        always {
            sh 'docker logout || true'
        }

        success {
            echo 'Docker image built and pushed successfully.'
        }

        failure {
            echo 'Pipeline failed.'
        }
    }
}
