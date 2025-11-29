pipeline {
    agent any
    
    parameters {
        choice(
            name: 'ENVIRONMENT',
            choices: ['dev', 'staging', 'production'],
            description: 'Select the deployment environment'
        )
        string(
            name: 'VERSION',
            defaultValue: '1.0.0',
            description: 'Enter the build version number'
        )
        booleanParam(
            name: 'RUN_TESTS',
            defaultValue: true,
            description: 'Run automated tests?'
        )
        booleanParam(
            name: 'DEPLOY',
            defaultValue: false,
            description: 'Deploy after successful build?'
        )
        text(
            name: 'RELEASE_NOTES',
            defaultValue: 'No release notes provided',
            description: 'Enter release notes for this build'
        )
    }
    
    stages {
        stage('Checkout') {
            steps {
                echo "━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"
                echo "📦 Checking out code from GitHub..."
                echo "Branch: ${env.GIT_BRANCH}"
                echo "Commit: ${env.GIT_COMMIT}"
                echo "━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"
            }
        }
        
        stage('Build Info') {
            steps {
                echo "━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"
                echo "🔧 BUILD CONFIGURATION"
                echo "━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"
                echo "Environment: ${params.ENVIRONMENT}"
                echo "Version: ${params.VERSION}"
                echo "Run Tests: ${params.RUN_TESTS}"
                echo "Deploy: ${params.DEPLOY}"
                echo "━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"
                echo "📝 Release Notes:"
                echo "${params.RELEASE_NOTES}"
                echo "━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"
            }
        }
        
        stage('Build') {
            steps {
                echo "🔨 Building version ${params.VERSION}..."
                echo "Target environment: ${params.ENVIRONMENT}"
                sleep 3
                echo "✓ Build completed successfully!"
            }
        }
        
        stage('Test') {
            when {
                expression { params.RUN_TESTS == true }
            }
            steps {
                echo "🧪 Running automated tests..."
                sleep 2
                echo "✓ All tests passed!"
            }
        }
        
        stage('Deploy') {
            when {
                expression { params.DEPLOY == true }
            }
            steps {
                echo "🚀 Deploying to ${params.ENVIRONMENT}..."
                sleep 3
                
                script {
                    if (params.ENVIRONMENT == 'production') {
                        echo "⚠️  WARNING: Deploying to PRODUCTION!"
                        echo "Version ${params.VERSION} is now LIVE!"
                    } else {
                        echo "✓ Deployed to ${params.ENVIRONMENT} environment"
                    }
                }
            }
        }
    }
    
    post {
        success {
            echo "━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"
            echo "✅ PIPELINE COMPLETED SUCCESSFULLY!"
            echo "━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"
            echo "Version: ${params.VERSION}"
            echo "Environment: ${params.ENVIRONMENT}"
            echo "Tests Run: ${params.RUN_TESTS}"
            echo "Deployed: ${params.DEPLOY}"
            echo "━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"
        }
        failure {
            echo "❌ Pipeline failed!"
        }
    }
}
