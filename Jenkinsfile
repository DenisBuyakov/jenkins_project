def suiteRunId = UUID.randomUUID().toString()
pipeline {
    agent none
        environment {
             AWS_S3_BUCKET = credentials('aws-s3-bucket')
        }
    stages {
                            stage('Build') {
                                        agent {
                                            dockerfile {
                                                filename 'Dockerfile'
                                                dir 'docker'
                                                //             reuseNode true
                                                //             args '-v $WORKSPACE:/tmp/project_${suiteRunId}'
                                                additionalBuildArgs '--build-arg version=1.0.0 --build-arg suite_run_id=${suiteRunId}'
                                                //             args '-v /tmp:/tmp'
                                                //             label "build-image"
                                            }
                                        }
                                steps {
                                    echo "Running ${env.BUILD_ID}"
                                    sh 'node --version'
                                    sh 'npm install'
                                }
                            }
        stage('build and test the project') {
            parallel {
                stage('Code analyse') {
                            agent {
                                dockerfile {
                                    filename 'Dockerfile'
                                    dir 'docker'
                                    //             reuseNode true
                                    //             args '-v $WORKSPACE:/tmp/project_${suiteRunId}'
                                    additionalBuildArgs '--build-arg version=1.0.0 --build-arg suite_run_id=${suiteRunId}'
                                    //             args '-v /tmp:/tmp'
                                    //             label "build-image"
                                }
                            }
                    steps {
                        sh 'npm run linter'
                    }
                }
                stage('Test') {
                            agent {
                                dockerfile {
                                    filename 'Dockerfile'
                                    dir 'docker'
                                    //             reuseNode true
                                    //             args '-v $WORKSPACE:/tmp/project_${suiteRunId}'
                                    additionalBuildArgs '--build-arg version=1.0.0 --build-arg suite_run_id=${suiteRunId}'
                                    //             args '-v /tmp:/tmp'
                                    //             label "build-image"
                                }
                            }
                    steps {
                        sh 'npm run test'
                    }
                }
            }
        }
        stage('upload to s3') {
            agent any
            steps {
                echo "WORKSPACE: ${WORKSPACE}"
//                 input message: 'Upload? (Click "Proceed" to continue)'
//                 withEnv(["ANOTHER_ENV_VAR="]) {
//                     echo "ANOTHER_ENV_VAR = ${env.ANOTHER_ENV_VAR}"
//                 }
//
//                 env.AWS_ACCESS_KEY_ID = creds['accessKeyId']
//                 env.AWS_SECRET_ACCESS_KEY = creds['secretAccessKey']
//                 env.AWS_DEFAULT_REGION = 'us-east-1' // or whatever
//                 sh '''
//                 export AWS_ACCESS_KEY_ID=AKIAIOSFODNN7EXAMPLE
//                 export AWS_SECRET_ACCESS_KEY=wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
//                 export AWS_DEFAULT_REGION=us-west-2
//                 '''
//
                sh 'aws s3 rm s3://$AWS_S3_BUCKET/jenkins-project-temp --recursive'
                sh 'aws s3 cp $WORKSPACE s3://$AWS_S3_BUCKET/jenkins-project-temp --recursive'
                sh 'aws s3 rm s3://$AWS_S3_BUCKET/jenkins-project --recursive'
                sh 'aws s3 mv s3://$AWS_S3_BUCKET/jenkins-project-temp/ s3://$AWS_S3_BUCKET/jenkins-project/ --recursive'
                sh 'aws s3 ls s3://$AWS_S3_BUCKET/jenkins-project'
//                 sh "aws s3 cp ${WORKSPACE}/public s3://${env.AWS_S3_BUCKET} --recursive"
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
