pipeline {
    agent any

    environment {
        IMAGE_NAME = 'webapp'
        DOCKER_REPO = 'rajeshgramam/jenkins_learn'
        IMAGE_TAG = 'latest'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Rajesh-Rajan-TF/jenkins-learn.git'
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
                withDockerRegistry([credentialsId: 'dockerhub-cred', url: '']) {
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
