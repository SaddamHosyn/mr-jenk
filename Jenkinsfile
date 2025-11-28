pipeline {
    agent any
    
    parameters {
        choice(name: 'ENVIRONMENT', choices: ['dev', 'staging', 'production'], description: 'Select deployment environment')
        string(name: 'VERSION', defaultValue: '1.0.0', description: 'Version number')
    }
    
    stages {
        stage('Checkout') {
            steps {
                echo "Checking out code..."
                echo "Branch: ${env.GIT_BRANCH}"
            }
        }
        
        stage('Build') {
            steps {
                echo "Building version ${params.VERSION}..."
                echo "Environment: ${params.ENVIRONMENT}"
                sleep 2
                echo "Build completed!"
            }
        }
        
        stage('Test') {
            steps {
                echo "Running tests..."
                sleep 2
                echo "All tests passed!"
            }
        }
        
        stage('Deploy') {
            steps {
                echo "Deploying to ${params.ENVIRONMENT}..."
                sleep 2
                echo "Deployment successful!"
            }
        }
    }
    
    post {
        success {
            echo "✓ Pipeline completed successfully!"
            echo "Version ${params.VERSION} deployed to ${params.ENVIRONMENT}"
        }
        failure {
            echo "✗ Pipeline failed!"
        }
    }
}
