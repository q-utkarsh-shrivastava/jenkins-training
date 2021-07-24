pipeline {
    agent {
        label 'jenkins-cloud-gcp'
    }

    environment {
        CREDENTIALS_ID ='srinibasmisra-gcp-trials-01'
        BUCKET = 'srinibasmisra-gcp-trials-01-jenkins-training'
        PATTERN = 'build/*'
    }

    tools {
        nodejs 'node-tool'
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
                    pattern: env.PATTERN])
            }
        }
    }
}