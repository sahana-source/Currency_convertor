pipeline {
    agent any

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    echo "Listing files in the workspace"
                    ls -la
                    echo "Node.js and npm versions:"
                    node --version
                    npm --version
                    ls -la
                '''
            }
        }

        stage('Test') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    echo "Installing dependencies..."
                    npm install
                    echo "Checking for test file..."
                    test -f src/CalculatorTest.java || echo "Test file not found!"
                    echo "Running tests..."
                '''
            }
        }

        stage('E2E') {
            agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.39.0-jammy'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    echo "Installing serve package..."
                    npm install serve
                    echo "Starting static server..."
                    node_modules/.bin/serve -s build &
                    sleep 10
                    echo "Running Playwright tests..."
                    npx playwright test 
                '''
            }
        }
    }

    post {
        always {
            script {
                try {
                    // Collect JUnit test results
                    junit 'jest-result/junit.xml' // Adjust path to match your test results directory
                } catch (Exception e) {
                    echo "No test results found: ${e.getMessage()}"
                }
            }
        }
        unstable {
            echo "Build marked as UNSTABLE. Check test results or warnings."
        }
    }
}
