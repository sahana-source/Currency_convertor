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
                    test -f src/CalculatorTest.java
                    echo "Running tests..."
                '''
            }
        }
    }

    post {
        always {
            echo "Publishing test results"
            junit 'test-result/junit.xml'  // Ensure this points to the correct file
        }
    }
}
