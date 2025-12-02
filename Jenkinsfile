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
        choice(
            name: 'BUILD_TYPE',
            choices: ['Standard', 'Hotfix', 'Release'],
            description: 'Type of build'
        )
        booleanParam(
            name: 'SIMULATE_FAILURE',
            defaultValue: false,
            description: 'Simulate a build failure for testing?'
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
        
        stage('Security & Access Control') {
            steps {
                echo "━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"
                echo "🔐 SECURITY & ACCESS CONTROL"
                echo "━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"
                echo ""
                echo "✓ User Roles Configured:"
                echo "  • Administrator: Full system access"
                echo "  • Developer: Can build, configure jobs"
                echo "  • Viewer: Read-only access"
                echo ""
                echo "✓ Authorization: Matrix-based security enabled"
                echo "✓ Authentication: Jenkins user database"
                echo "✓ Permissions: Role-based access control (RBAC)"
                echo ""
                echo "✓ Credentials Management:"
                echo "  • Secrets stored in Jenkins Credentials Store"
                echo "  • API keys accessed via withCredentials() blocks"
                echo "  • Passwords encrypted at rest"
                echo "  • No sensitive data in source code or logs"
                echo ""
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
                echo "Build Type: ${params.BUILD_TYPE}"
                echo "Simulate Failure: ${params.SIMULATE_FAILURE}"
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
                
                script {
                    if (params.SIMULATE_FAILURE == true) {
                        echo "Running tests..."
                        echo "Test 1: PASSED ✓"
                        echo "Test 2: PASSED ✓"
                        echo "Test 3: FAILED ✗"
                        error("Tests failed! 1 out of 3 tests failed.")
                    } else {
                        sleep 2
                        echo "✓ All tests passed!"
                    }
                }
            }
        }
        
        stage('Quality Gate') {
            steps {
                echo "🔍 Running quality checks..."
                
                script {
                    if (params.SIMULATE_FAILURE == true && params.RUN_TESTS == false) {
                        echo "❌ Simulating build failure for testing..."
                        echo "ERROR: Code quality threshold not met!"
                        error("Build failed: Quality gate check failed!")
                    } else {
                        echo "✓ Quality gate passed!"
                    }
                }
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
            echo "Build Type: ${params.BUILD_TYPE}"
            echo "━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"
            
            script {
                def buildInfo = """
            ✅ BUILD SUCCESS
            
            Job: ${env.JOB_NAME}
            Build Number: ${env.BUILD_NUMBER}
            Version: ${params.VERSION}
            Environment: ${params.ENVIRONMENT}
            Build Type: ${params.BUILD_TYPE}
            Tests Run: ${params.RUN_TESTS}
            Deployed: ${params.DEPLOY}
            
            Console Output: ${env.BUILD_URL}console
            
            Release Notes:
            ${params.RELEASE_NOTES}
            """
                
                echo "📧 Notification would be sent:"
                echo buildInfo
            }
        }
        
        failure {
            echo "━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"
            echo "❌ PIPELINE FAILED!"
            echo "━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"
            
            script {
                def failureInfo = """
            ❌ BUILD FAILED
            
            Job: ${env.JOB_NAME}
            Build Number: ${env.BUILD_NUMBER}
            Version: ${params.VERSION}
            Environment: ${params.ENVIRONMENT}
            
            Console Output: ${env.BUILD_URL}console
            
            Please check the logs and fix the issue.
            """
                
                echo "📧 Failure notification would be sent:"
                echo failureInfo
            }
        }
        
        always {
            echo "━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"
            echo "📊 Build completed at: ${new Date()}"
            echo "Duration: ${currentBuild.durationString}"
            echo "Result: ${currentBuild.result}"
            echo "━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"
        }
    }
}
