def suiteRunId = UUID.randomUUID().toString()
pipeline {
    agent {
        dockerfile {
            filename 'Dockerfile'
            dir 'docker'
            reuseNode true
//             args '-v $WORKSPACE:/tmp/project_${suiteRunId}'
            additionalBuildArgs  '--build-arg version=1.0.0 --build-arg suite_run_id=${suiteRunId}'

        }
    }
    environment {
        AWS_S3_BUCKET = credentials('aws-s3-bucket')
    }
    stages {
        stage('Build') {
            steps {
                echo "Running ${env.BUILD_ID}"
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
        stage('upload to s3') {
//             agent any
            steps {
                echo "WORKSPACE: ${WORKSPACE}"
                input message: 'Upload? (Click "Proceed" to continue)'
                sh "aws s3 cp ${WORKSPACE}/public s3://${env.AWS_S3_BUCKET} --recursive"
            }
        }
//         stage('zip to s3') {
//             environment {
//                 AWS_DEFAULT_REGION=<region of bucket>
//                 AWS_ACCESS_KEY_ID=<aws id>
//                 AWS_SECRET_ACCESS_KEY=<your s3 access key>
//                 STACK_NAME = 'sam-app-beta-stage'
//                 S3_BUCKET = 'sam-jenkins-demo-us-west-2-user1'
//             }
//             steps {
//                 input message: 'Publish? (Click "Proceed" to continue)'
//             }
//         withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'deploytos3', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
//             sh 'aws s3 cp public s3://<bucket-name> --recursive'
//             sh "aws s3 ls"
//             sh "aws s3 cp addressbook_main/target/addressbook.war s3://cloudyeticicd/"
//         }
    }
}
