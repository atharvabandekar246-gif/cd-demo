pipeline {
    agent any
    environment {
        IMAGE = 'atharva6002/flask-demo'
    }
    stages {
        stage('Checkout') {
            steps { checkout scm }
        }
        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                    kubectl set image deployment/web-deploy web=$IMAGE:latest
                    kubectl rollout restart deployment/web-deploy
                    kubectl rollout status deployment/web-deploy --timeout=120s
                '''
            }
        }
        stage('Verify') {
            steps {
                sh 'kubectl get pods'
            }
        }
    }
    post {
        success { echo 'Deployment succeeded.' }
        failure {
            echo 'Deployment failed, rolling back.'
            sh 'kubectl rollout undo deployment/web-deploy || true'
        }
    }
}
