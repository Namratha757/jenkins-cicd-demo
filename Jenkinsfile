pipeline {
    agent any
    
    options {
        // This stops Jenkins from trying to do the broken automatic checkout
        skipDefaultCheckout()
    }

    environment {
        REGISTRY = "trialm1bhcd.jfrog.io/docker-trial"
    }

    stages {
        stage('Clean Checkout') {
            steps {
                // Manually clean and clone the repo
                deleteDir() 
                checkout scm
            }
        }

        stage('Build Images') {
            steps {
                sh 'docker build -t backend:v1.0 ./backend'
                sh 'docker build -t frontend:v1.0 ./frontend'
            }
        }

        stage('Tag Images') {
            steps {
                sh "docker tag backend:v1.0 ${REGISTRY}/backend:v1.0"
                sh "docker tag frontend:v1.0 ${REGISTRY}/frontend:v1.0"
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'jfrog-creds', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh 'echo $PASS | docker login trialm1bhcd.jfrog.io -u $USER --password-stdin'
                }
            }
        }

        stage('Push to JFrog') {
            steps {
                sh "docker push ${REGISTRY}/backend:v1.0"
                sh "docker push ${REGISTRY}/frontend:v1.0"
            }
        }

        stage('Deploy to Minikube') {
            steps {
                sh 'kubectl apply -f k8s/'
            }
        }
    }
}