pipeline {
    agent any

    environment {
        AWS_REGION='ap-south-1'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
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
