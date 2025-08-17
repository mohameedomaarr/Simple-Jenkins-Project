pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-credentials') // Jenkins credential ID
        IMAGE_NAME = "simple-project-img"                             // Local image name
        DOCKERHUB_REPO = "mohameedomaarr/simple-project-img"          // Docker Hub repository
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t $IMAGE_NAME:latest ."
                }
            }
        }

        stage('Login to Docker Hub') {
            steps {
                script {
                    sh """
                    echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin
                    """
                }
            }
        }

        stage('Tag Docker Image') {
            steps {
                script {
                    sh "docker tag $IMAGE_NAME:latest $DOCKERHUB_REPO:latest"
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    sh "docker push $DOCKERHUB_REPO:latest"
                }
            }
        }
    }

    post {
        always {
            sh "docker logout"
        }
    }
}

