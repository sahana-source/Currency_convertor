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
                    npm test
                    
                    echo "Ensure test results are available in test-result directory"
                    mkdir -p test-result
                    # Move test result XML files to test-result/ (update path if needed)
                    mv path/to/generated-test-results/*.xml test-result/ || echo "No test results found."
                '''
            }
        }
    }

    post {
        always {
            echo "Publishing test results"
            junit 'test-result/junit.xml'
        }
    }
}
