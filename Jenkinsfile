pipeline {
    agent any

    stages {
        stage('Test') {
            agent {
                docker {
                    image 'node:18-alpine'
                }
            }
            steps {
                echo '====================--START TEST--===================='
                sh '''
                    echo "Checking Node.js and npm versions inside container..."
                    node -v
                    npm -v
		'''
                echo '====================--TEST complete--===================='
            }
        }
    }
}
