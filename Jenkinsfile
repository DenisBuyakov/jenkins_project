pipeline {
    agent {
        docker {
            image 'node:14-alpine'
            args '-p 3010:3010'
        }
    }
    stages {
        stage('Build') {
            steps {
                sh 'npm install'
            }
        }
        stage('Test') {
            steps {
                sh 'npm run test'
            }
        }
    }
}
