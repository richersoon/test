pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                emailext (
                    subject: "${env.JOB_NAME} - Build# ${env.BUILD_NUMBER} - Building!'",
                    recipientProviders: [[$class: 'DevelopersRecipientProvider', $class: 'RequesterRecipientProvider']],
                    body: '$DEFAULT_CONTENT',               
                    mimeType: 'text/html',
                )
                checkout scm
            }
        }
        stage('Build') {
            steps {
                sh "ls -la"
                sh "./gradlew clean build"
            }
        }
        stage('Results') {
            steps {
                junit '**/test-results/test/TEST-*.xml' 
            }
        }
    }
    post {
        success {           
          emailext (
            subject: "${env.JOB_NAME} - Build# ${env.BUILD_NUMBER} - Successful!'",
            recipientProviders: [[$class: 'DevelopersRecipientProvider', $class: 'RequesterRecipientProvider']],
            body: '',  
            attachLog: true           
          )
        }

        failure {
          emailext (
            subject: "${env.JOB_NAME} - Build# ${env.BUILD_NUMBER} - Successful!'",
            recipientProviders: [[$class: 'DevelopersRecipientProvider', $class: 'RequesterRecipientProvider', $class: 'FailingTestSuspectsRecipientProvider']],  
            body: '',
            attachLog: true            
          )
        }
    }
}
