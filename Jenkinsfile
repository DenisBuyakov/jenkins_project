pipeline {
    agent {
        docker {
            image 'node:14-alpine'
        }
    }
    environment {
        AWS_S3_BUCKET = credentials('aws-s3-bucket')
    }
    stages {
        stage('Build') {
            steps {
                echo "aws S3 bucket: ${env.AWS_S3_BUCKET}"
                sh 'node --version'
                sh 'npm install'
            }
        }
        stage('Code analyse') {
            steps {
                sh 'npm run linter'
            }
        }
        stage('Test') {
            steps {
                sh 'npm run test'
            }
        }
    }
}
