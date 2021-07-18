pipeline {
    agent any;

    environment {
        CREDENTIALS_ID ='srini-sandbox-env'
        BUCKET = 'jenkins-training'
        PATTERN = 'build/*'
        STRIP_PATH = 'build/'
    }

    tools {
        nodejs 'jenkins-demo-node'
    }

    stages {

        stage('Initialize') {
            steps {
                sh '''
                    npm install
                '''
            }
        }

        stage('Unit Tests') {
            steps {
                sh '''
                    npm run test -- --watchAll=false
                '''
            }
        }

        stage('Build') {
            steps {
                sh '''
                    npm run build
                '''
            }
        }

        stage('Move to GCS') {
            steps{

                step([$class: 'ClassicUploadStep',
                    credentialsId: env.CREDENTIALS_ID, 
                    bucket: "gs://${env.BUCKET}",
                    pattern: env.PATTERN,
                    stripPathPrefix: env.STRIP_PATH])
            }
        }
    }
}