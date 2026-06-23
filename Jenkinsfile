pipeline {
    agent any

    environment {
        AWS_REGION = 'ap-south-1'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('ECR Login') {
            steps {
                sh '''
                aws ecr get-login-password --region $AWS_REGION | \
                docker login --username AWS --password-stdin \
                429965676677.dkr.ecr.ap-south-1.amazonaws.com
                '''
            }
        }

        stage('Build') {
            steps {
                sh 'docker compose build'
            }
        }

        stage('Push Images') {
            steps {
                sh '''
                docker push 429965676677.dkr.ecr.ap-south-1.amazonaws.com/streamingapp-auth:latest
                docker push 429965676677.dkr.ecr.ap-south-1.amazonaws.com/streamingapp-frontend:latest
                docker push 429965676677.dkr.ecr.ap-south-1.amazonaws.com/streamingapp-chat:latest
                docker push 429965676677.dkr.ecr.ap-south-1.amazonaws.com/streamingapp-admin:latest
                docker push 429965676677.dkr.ecr.ap-south-1.amazonaws.com/streamingapp-streaming:latest
                '''
            }
        }

    }
}
