pipeline {
    agent any

    environment {
        // Docker Hub credentials (if pushing images)
        DOCKER_HUB_CREDENTIALS = 'dockerhub-id'  // Replace with your Jenkins credentials ID
        DOCKER_REGISTRY = 'your-dockerhub-username'
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout code from your GitHub repo
                git branch: 'main', url: 'https://github.com/Arsh-Git1/fusionpact.git'
            }
        }

        stage('Build Docker Images') {
            steps {
                script {
                    // Build backend Docker image
                    sh 'docker build -t backend-image ./backend'

                    // Build frontend Docker image
                    sh 'docker build -t frontend-image ./frontend'
                }
            }
        }

        stage('Push Docker Images') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', DOCKER_HUB_CREDENTIALS) {
                        sh 'docker tag backend-image $DOCKER_REGISTRY/backend-image:latest'
                        sh 'docker tag frontend-image $DOCKER_REGISTRY/frontend-image:latest'
                        sh 'docker push $DOCKER_REGISTRY/backend-image:latest'
                        sh 'docker push $DOCKER_REGISTRY/frontend-image:latest'
                    }
                }
            }
        }

        stage('Deploy to VM') {
            steps {
                // SSH into your GCP VM and deploy using docker-compose
                sshagent(['gcp-vm-ssh-key']) {
                    sh '''
                    ssh -o StrictHostKeyChecking=no user@34.100.223.142 << EOF
                    cd ~/fusionpact-devops-challenge-solution
                    docker compose down
                    docker compose up -d
                    EOF
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
