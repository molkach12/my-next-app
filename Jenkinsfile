pipeline {
    agent any

    environment {
        DOCKERHUB_USER = 'ton-user'
        DOCKERHUB_PASS = credentials('dockerhub-creds')
        IMAGE_NAME = 'ton-user/mon-app'
    }

    stages {
        stage('Checkout') {
            steps {
                echo '📥 Cloning repository...'
                checkout scm
            }
        }

        stage('Test & Scan') {
            steps {
                echo '🧪 Running tests & security scans...'
                sh 'npm install && npm test'
                sh 'npm audit || true' // ne bloque pas le pipeline
            }
        }

        stage('Build Docker Image') {
            steps {
                echo '🐳 Building Docker image...'
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Push Image to DockerHub') {
            steps {
                echo '🚀 Pushing image...'
                sh 'echo $DOCKERHUB_PASS | docker login -u $DOCKERHUB_USER --password-stdin'
                sh 'docker push $IMAGE_NAME'
            }
        }

        stage('Deploy to Remote') {
            steps {
                echo '📦 Deploying on remote server...'
                // tu peux utiliser ssh ou docker run ici
                sh 'docker run -d -p 3000:3000 $IMAGE_NAME'
            }
        }

        stage('Health Check') {
            steps {
                echo '🩺 Verifying health...'
                sh 'curl -f http://localhost:3000/health || exit 1'
            }
        }

        stage('Notify') {
            steps {
                echo '📣 Notifying team...'
                // Exemple : Slack, Discord, Email, etc.
            }
        }
    }

    post {
        failure {
            echo '⚠️ Something went wrong, rolling back...'
            // Exemple : rollback docker container
        }
    }
}
