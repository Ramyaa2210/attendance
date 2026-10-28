
pipeline {
    agent any

    stages {
        stage('1. Checkout') {
            steps {
                checkout scm
            }
        }

        stage('2. Install Dependencies') {
            steps {
                bat 'python -m pip install -r requirements.txt'
            }
        }

        stage('3. Run Tests') {
            steps {
                bat 'python -m pytest -v'
            }
        }

        stage('4. Build Docker Image') {
            steps {
                bat 'docker build -t ramyagarla/attendance-app:latest .'
            }
        }

        stage('5. Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-credentials',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    bat 'docker login -u "%DOCKER_USER%" -p "%DOCKER_PASS%"'
                    bat 'docker push ramyagarla/attendance-app:latest'
                }
            }
        }

        stage('6. Deploy to Kubernetes') {
            steps {
                bat 'kubectl apply -f k8s/deployment.yaml'
            }
        }

        stage('7. Verify Deployment') {
            steps {
                bat 'kubectl rollout status deployment/attendance-app'
                bat 'kubectl get pods'
                bat 'kubectl get services'
            }
        }
    }
}
