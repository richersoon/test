pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                checkout scm
                def GIT_COMMIT = sh (
                    script: 'git rev-parse HEAD',
                    returnStdout: true
                ).trim()
                
                emailext (
                    subject: "${env.JOB_NAME} - Build# ${env.BUILD_NUMBER} - Building!",
                    recipientProviders: [[$class: 'DevelopersRecipientProvider', $class: 'RequesterRecipientProvider']],
                    body: '$GIT_COMMIT',               
                    mimeType: 'text/html',
                )
            }
        }
        stage('Build') {
            steps {
                sh "./gradlew clean build"
            }
        }
    }
    post {
        success {  
          emailext (
            subject: "${env.JOB_NAME} - Build# ${env.BUILD_NUMBER} - Successful!",
            recipientProviders: [[$class: 'DevelopersRecipientProvider', $class: 'RequesterRecipientProvider']],
            body: '',  
            attachLog: true           
          )
        }

        failure {
          emailext (
            subject: "${env.JOB_NAME} - Build# ${env.BUILD_NUMBER} - Successful!",
            recipientProviders: [[$class: 'DevelopersRecipientProvider', $class: 'RequesterRecipientProvider', $class: 'FailingTestSuspectsRecipientProvider']],  
            body: '',
            attachLog: true            
          )
        }
    }
}
