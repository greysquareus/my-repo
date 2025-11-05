pipeline {
    agent any
    
    stages {
        stage('Pre_test') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                echo '====================--START PRE_TEST--===================='
                sh '''
                    ls -la
                    echo "Checking Node.js and npm versions inside container..."
                    node -v
                    npm -v
                '''
                echo '====================--PRE_TEST complete--===================='
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
                    mkdir -p .npm-cache
                    export NPM_CONFIG_CACHE=$(pwd)/.npm-cache
                    npm ci
                    npm run build
                    ls -la
                '''
                echo '====================--BUILD complete--===================='
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
                echo '====================--TEST--===================='
                sh '''
                    echo 'test wb soon'
                '''
                echo '====================--TEST complete--===================='
            }
        }
    }
    
    post {
        success {
            echo '✅ Build completed successfully!'
        }
        failure {
            echo '❌ Build failed. Check logs for details.'
        }
    }
}
