pipeline {
    agent any

    stages {
        stage('Test') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                echo '====================--START TEST--===================='
                sh '''
                    ls -la
                    echo "Checking Node.js and npm versions inside container..."
                    node -v
                    npm -v
                '''
                echo '====================--TEST complete--===================='
            }
        }

        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                echo '====================--START BUILD--===================='
                sh '''
                    rm -rf node_modules
                    npm cache clean --force
                    npm ci
                    npm run build
                    ls -la
                '''
            }
        }
    }
}

    post {
        always {
            echo '✅ Pipeline finished (cleaning up workspace)...'
            cleanWs()
        }
        failure {
            echo '❌ Build failed. Check logs for details.'
        }
    }

