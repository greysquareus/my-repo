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
                    image 'mcr.microsoft.com/playwright:v1.46.0-focal'
                    reuseNode true
                }
            }
            steps {
                echo '====================--E2E TEST START--===================='
                sh '''
                    # Обновляем Playwright до версии, соответствующей контейнеру
                    npm install -D @playwright/test@1.46.0

                    # Ставим serve для локального тест-сервера
                    npm install serve

                    # Запускаем сервер на фоне
                    npx serve -s build &
                    SERVER_PID=$!
                    
                    # Даем время серверу запуститься
                    sleep 15

                    # Запускаем тесты
                    npx playwright test

                    # Убиваем сервер после тестов
                    kill $SERVER_PID || true
                '''
                echo '====================--E2E TEST COMPLETE--===================='
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
                    npx serve -s build &
                    echo "Server started successfully"
                '''
                sleep 30
                echo '====================--DEPLOY COMPLETED--===================='
            }
        }
    }
    
    post {
        always {
            junit 'jest-results/junit.xml'
        }
        success {
            echo '✅ Build completed successfully!'
        }
        failure {
            echo '❌ Build failed. Check logs for details.'
        }
    }
}

