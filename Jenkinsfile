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
                    echo "Listing files in the workspace..."
                    ls -la
                    echo "Checking Node.js and npm versions..."
                    node --version
                    npm --version
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
                    echo "Running unit tests..."
                    npm test || echo "Unit tests failed."
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
                    echo "Installing dependencies..."
                    npm install
                    echo "Building the project..."
                    npm run build
                    echo "Starting static server..."
                    serve -s build &
                    sleep 10
                    echo "Running Playwright tests..."
                    npx playwright test tests/ || echo "E2E tests failed."
                '''
            }
        }
    }

    post {
        always {
            script {
                try {
                    echo "Collecting test results..."
                    junit 'jest-result/junit.xml' // Update path to your test results
                } catch (Exception e) {
                    echo "No test results found: ${e.getMessage()}"
                }
            }
        }
        unstable {
            echo "Build marked as UNSTABLE. Check test results or warnings."
        }
        failure {
            echo "Build failed. Investigate the logs for details."
        }
    }
}
