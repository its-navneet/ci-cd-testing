pipeline {
    agent any
    
    environment {
        NODE_ENV = 'production'
        PORT = '3000'
    }
    
    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out source code...'
                checkout scm
            }
        }
        
        stage('Install Dependencies') {
            steps {
                echo 'Installing dependencies...'
                sh 'npm install'
            }
        }
        
        stage('Lint') {
            steps {
                echo 'Running code quality checks...'
                // Placeholder for linting (can add ESLint later)
                sh 'echo "Linting passed"'
            }
        }
        
        stage('Test') {
            steps {
                echo 'Running tests...'
                sh 'npm test'
            }
        }
        
        stage('Build') {
            steps {
                echo 'Building application...'
                sh 'echo "Build completed"'
            }
        }
        
        stage('Deploy') {
            steps {
                echo 'Deploying application...'
                sh 'echo "Deployment completed"'
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
        always {
            echo 'Cleaning up...'
        }
    }
}

