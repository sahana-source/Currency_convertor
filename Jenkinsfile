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
                    if [ ! -f package.json ]; then
                        echo "Error: package.json not found!"
                        exit 1
                    fi
                    node --version
                    npm --version
                    npm ci || npm install
                    npm run build
                    ls -la
                '''
            }
        }
    }
}
