pipeline {
    agent any
    
    environment {
        DOCKERHUB_REPO = "khalidsaiyed/static-site"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/skhalidedrus-ops/Makaan-Application.git'
            }
        }

        stage('Build Image') {
            steps {
                sh """
                docker build -t ${DOCKERHUB_REPO}:latest .
                """
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub-creds',
                    usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                        sh """
                        echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                        """
                }
            }
        }

        stage('Push Image') {
            steps {
                sh "docker push ${DOCKERHUB_REPO}:latest"
            }
        }
    }

    post {
        success {
            echo "CI Pipeline completed — Image pushed to Docker Hub!"
        }
    }
}

