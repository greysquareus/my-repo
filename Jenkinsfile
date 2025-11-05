pipeline {
    agent any

        stage('Test') {
            agent {
                docker {
                    image 'node:18-alpine'  
                }
            }
            steps {
                sh '''
			apt update 
			apt install npm
			npm --version
		'''
            }
        }
    }
}
