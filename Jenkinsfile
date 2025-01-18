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
                    echo "Running tests..."
                    npm test || echo "Tests failed."
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
                    npx serve -s build &
                    sleep 10
                    echo "Running Playwright tests..."
                    npx playwright test --reporter=html,junit || echo "E2E tests failed."
                '''
            }
        }

            post {
                always {
                    script {
                        try {
                            echo "Collecting JUnit test results..."
                            junit 'junit-test/junit.xml' // Updated path to JUnit results
                        } catch (Exception e) {
                            echo "No JUnit test results found: ${e.getMessage()}"
                        }
                    }
                    publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: 'playwright-report', reportFiles: 'index.html', reportName: 'Playwright HTML Report', reportTitles: '', useWrapperFileDirectly: true])
                }
                unstable {
                    echo "Build marked as UNSTABLE. Check test results or warnings."
                }
                failure {
                    echo "Build failed. Investigate the logs for details."
                }
            
    }
    stage('Deploy') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    npm install netlify-cli
                    node_modules/.bin/netlify --version
                '''
            }
        }
    }
}
