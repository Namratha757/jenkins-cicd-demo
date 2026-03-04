pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/your-repo.git'
            }
        }

        stage('Build Backend Image') {
            steps {
                sh 'docker build -t your-docker-username/backend:v1.0 ./backend'
            }
        }

        stage('Build Frontend Image') {
            steps {
                sh 'docker build -t your-docker-username/frontend:v1.0 ./frontend'
            }
        }

        stage('Push to JFrog') {
            steps {
                sh 'docker push your-jfrog-repo/backend:v1.0'
                sh 'docker push your-jfrog-repo/frontend:v1.0'
            }
        }

        stage('Deploy to Minikube') {
            steps {
                sh 'kubectl apply -f k8s/'
            }
        }
    }
}