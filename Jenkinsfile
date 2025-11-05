pipeline {
    agent any

        stage('Test') {
            agent {
                docker {
                    image 'node:18-alpine'
                }
            }
            steps {
                echo '====================--START TEST--===================='
                sh '''
                        apt update -y
                        apt install npm -y
                        npm --version
                '''
                echo '====================--TEST complete--===================='
            }
        }
    }
