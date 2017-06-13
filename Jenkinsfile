pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                emailext body: '$DEFAULT_CONTENT', 
                    replyTo: '$DEFAULT_REPLYTO', 
                    subject: '$PROJECT_NAME - Build # $BUILD_NUMBER - $BUILD_STATUS!', 
                    to: '$DEFAULT_RECIPIENTS'
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
        def test123 = currentBuild.rawBuild.log();
        
        success {
          emailext (
            subject: "SUCCESSFUL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
            mimeType: 'text/html',  
            body: """<p>SUCCESSFUL: Job ${env.JOB_NAME} [${env.BUILD_NUMBER}]:</p>
            <p>Check console output at ${test123}</p>""",
            recipientProviders: [[$class: 'DevelopersRecipientProvider', $class: 'RequesterRecipientProvider']]
          )
        }

        failure {
          emailext (
            subject: "FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
            mimeType: 'text/html',  
            body: """<p>FAILED: Job ${env.JOB_NAME} [${env.BUILD_NUMBER}]:</p>
            <p>Check console output at ${env.BUILD_URL}</p>""",
            recipientProviders: [[$class: 'DevelopersRecipientProvider', $class: 'RequesterRecipientProvider', $class: 'FailingTestSuspectsRecipientProvider']]
          )
        }
    }
}
