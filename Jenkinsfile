pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                checkout scm
                script {
                    GIT_COMMIT = sh (
                        script: 'git diff HEAD^ HEAD',
                        returnStdout: true
                    ).trim()
                }  
                emailext (
                    subject: "${env.JOB_NAME} - Build# ${env.BUILD_NUMBER} - Building!",
                    recipientProviders: [[$class: 'DevelopersRecipientProvider'], [$class: 'RequesterRecipientProvider'], $class: 'ListRecipientProvider'],
                    body: "The following are the changes\n\n ${GIT_COMMIT}"
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
            recipientProviders: [[$class: 'DevelopersRecipientProvider'], [$class: 'RequesterRecipientProvider']],
            body: '',  
            attachLog: true           
          )
        }

        failure {
          emailext (
            subject: "${env.JOB_NAME} - Build# ${env.BUILD_NUMBER} - Successful!",
            recipientProviders: [[$class: 'DevelopersRecipientProvider'], [$class: 'RequesterRecipientProvider'], [$class: 'FailingTestSuspectsRecipientProvider']],  
            body: '',
            attachLog: true            
          )
        }
    }
}
