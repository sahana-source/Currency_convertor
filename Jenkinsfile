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
            script {
                try {
                    junit '**/target/surefire-reports/*.xml' // Adjust path as needed
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
