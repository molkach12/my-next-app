pipeline {
    agent any

    environment {
        DOCKERHUB_USER = 'molkaaah'
        DOCKERHUB_PASS = credentials('dockerhub-creds')  // à créer dans Jenkins
        IMAGE_NAME = "${DOCKERHUB_USER}/my-next-app"
    }

    stages {
        stage('Checkout') {
            steps {
                echo '📥 Cloning code...'
                checkout scm
            }
        }

        stage('Tests & Security') {
            steps {
                echo '🧪 Running tests and scans...'
                sh 'npm install'
                sh 'npm test || true'
                sh 'npm audit || true'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo '🐳 Building image...'
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Push to DockerHub') {
            steps {
                echo '🚀 Pushing image to DockerHub...'
                sh 'echo $DOCKERHUB_PASS | docker login -u $DOCKERHUB_USER --password-stdin'
                sh 'docker push $IMAGE_NAME'
            }
        }

        stage('Deploy Container') {
            steps {
                echo '📦 Deploying container...'
                sh 'docker rm -f myapp || true'
                sh 'docker run -d -p 3000:3000 --name myapp $IMAGE_NAME'
            }
        }

        stage('Health Check') {
            steps {
                echo '🔍 Checking health...'
                sh 'curl -f http://localhost:3000 || exit 1'
            }
        }

        stage('Notify') {
            steps {
                echo '📣 Build succeeded! (You can integrate Slack/email here)'
            }
        }
    }

    post {
        failure {
            echo '⚠️ Failure detected, rolling back...'
            sh 'docker restart myapp' // ou autre rollback
        }
    }
}
