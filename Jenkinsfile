pipeline {
  agent {
    dockerfile {
      filename 'Dockerfile'
      dir 'docker'
      reuseNode true
      additionalBuildArgs '--build-arg version=1.0.0 --build-arg suite_run_id=${suiteRunId}'
    }

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
      agent any
      steps {
        echo "WORKSPACE: ${WORKSPACE}"
        input 'Upload? (Click "Proceed" to continue)'
        sh "aws s3 cp ${WORKSPACE} s3://denis-jenkins/jenkins-project-temp --recursive"
      }
    }

  }
  environment {
    AWS_S3_BUCKET = credentials('aws-s3-bucket')
  }
}