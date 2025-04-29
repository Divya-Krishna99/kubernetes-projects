pipeline {
    agent any

    environment {
        IMAGE_NAME = "divyakrishna123/flask-app:v1"
    }

    stages {
        stage('Clone Repo') {
            steps {
                git 'https://github.com/Divya-Krishna99/kubernetes-projects.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Push to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh """
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                        docker push $IMAGE_NAME
                    """
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f flask-deployment.yml'
            }
        }
    }
}
