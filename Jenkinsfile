pipeline {
    agent any
    stages {
        stage('Checkout') {
           
                checkout scm
                emailext (
                    subject: "${env.JOB_NAME} - Build# ${env.BUILD_NUMBER} - Building!",
                    recipientProviders: [[$class: 'DevelopersRecipientProvider', $class: 'RequesterRecipientProvider']],
                    body: '$GIT_COMMIT',               
                    mimeType: 'text/html',
                )
            
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
