pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                checkout scm
                script {
                    GIT_DIFF = sh (
                        script: 'git diff HEAD^ HEAD',
                        returnStdout: true
                    ).trim()
                }
                emailext (
                    subject: "${env.JOB_NAME} - Build# ${env.BUILD_NUMBER} - Building!",
                    to: '$DEFAULT_RECIPIENTS',
                    recipientProviders: [[$class: 'DevelopersRecipientProvider'], [$class: 'RequesterRecipientProvider']],
                    body: "The following are the changes\n\n ${GIT_DIFF}"
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
            to: '$DEFAULT_RECIPIENTS',
            recipientProviders: [[$class: 'DevelopersRecipientProvider'], [$class: 'RequesterRecipientProvider']],
            body: '',  
            attachLog: true           
          )
        }

        failure {
          emailext (
            subject: "${env.JOB_NAME} - Build# ${env.BUILD_NUMBER} - Successful!",
            to: '$DEFAULT_RECIPIENTS',
            recipientProviders: [[$class: 'DevelopersRecipientProvider'], [$class: 'RequesterRecipientProvider'], [$class: 'FailingTestSuspectsRecipientProvider']],  
            body: '',
            attachLog: true            
          )
        }
    }
}
