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
                    if [ -f package-lock.json ]; then
                        npm ci
                    else
                        npm install
                    fi
                    npm run build
                    ls -la
                '''
            }
        }
    }
}
