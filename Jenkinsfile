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
                    ls -la
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
                    echo "Running tests..."
                    npm install
                    npm test
                '''
            }
        }
    }

    post {
        always{
            junit 'test-result/junit.xml'
        }
        failure {
            echo 'Tests failed! Check the logs and fix the issues.'
        }
        success {
            echo 'Build and tests succeeded!'
        }
    }
    
}
