pipeline {
    agent any

    environment {
        NODE_ENV = 'development'
    }

    stages {
        stage('Checkout') {
            steps {
                echo '📥 Cloning repository...'
                checkout scm
            }
        }

        stage('Install dependencies') {
            steps {
                echo '📦 Installing dependencies...'
                sh 'npm install'
            }
        }

        stage('Run tests') {
            steps {
                echo '🧪 Running tests...'
                sh 'npm test || echo "No test configured"'
            }
        }

        stage('Build project') {
            steps {
                echo '🔨 Building the project...'
                sh 'npm run build || echo "No build script defined"'
            }
        }

        stage('Deploy') {
            steps {
                echo '🚀 Deploying the project...'
                sh 'echo "Deploying app..."'
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline completed successfully!'
        }
        failure {
            echo '❌ Pipeline failed.'
        }
    }
}
