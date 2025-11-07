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
                    test -f build/index.html
                    npm test
                '''
                echo '====================--TEST complete--===================='
            }
        }



        stage('End-to-end Test') {
            agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.56.1-noble'
                    reuseNode true
		    
                }
            }
            steps {
                echo '====================--TEST--===================='
                sh '''
                    npm install serve
                    node_modules/.bin/serve -s build
                    npx playwright test
                '''
                echo '====================--TEST complete--===================='
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
                echo '====================--DEPLOY--===================='
                sh '''
                    npx serve -s build -l 5050
                    echo "Server runned successfully"
                '''
                sleep 30
                echo '====================--DEPLOY-COMPLETED--===================='
            }
        }
    }
    
    post {
        always {
                junit 'test-results/junit.xml'
        }
        success {
            echo '✅ Build completed successfully!'
        }
        failure {
            echo '❌ Build failed. Check logs for details.'
        }
    }
}
