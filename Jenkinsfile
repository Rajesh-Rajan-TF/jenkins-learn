pipeline {
    agent any

    environment {
        IMAGE_NAME = 'webapp'
        DOCKER_REPO = 'your-dockerhub-username'
        IMAGE_TAG = 'latest'
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://your-repo-url/webapp.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKER_REPO}/${IMAGE_NAME}:${IMAGE_TAG}")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                withDockerRegistry([credentialsId: 'docker-hub-credentials', url: '']) {
                    script {
                        docker.image("${DOCKER_REPO}/${IMAGE_NAME}:${IMAGE_TAG}").push()
                    }
                }
            }
        }

        stage('Deploy to Minikube') {
            steps {
                sh '''
                kubectl delete deployment webapp --ignore-not-found
                kubectl delete service webapp-service --ignore-not-found
                kubectl apply -f deployment.yaml
                kubectl apply -f service.yaml
                '''
            }
        }
    }
}
