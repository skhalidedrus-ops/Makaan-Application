pipeline {
    agent any

    environment {
        DOCKERHUB_REPO = "khalidsaiyed/static-site"
        DOCKER_BUILDKIT = "0"
    }

    stages {

        stage('Checkout') {
            steps {
                git url: 'https://github.com/skhalidedrus-ops/Makaan-Application.git', branch: 'main'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                    docker build -t ${DOCKERHUB_REPO}:latest .
                '''
            }
        }

        stage('Docker Hub Login') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'docker',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                        docker login -u "$DOCKER_USER" --password-stdin <<EOF
$DOCKER_PASS
EOF
                    '''
                }
            }
        }

        stage('Push Image') {
            steps {
                sh '''
                    docker push ${DOCKERHUB_REPO}:latest
                '''
            }
        }
    }

    post {
        success {
            echo "🎉 CI Success — Image built & pushed to Docker Hub!"
        }
        failure {
            echo "❌ Pipeline Failed!"
        }
    }
}
