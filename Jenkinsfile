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
       success {
            emailext body: 'Logs: \n\n$currentBuild.rawBuild.log', 
               replyTo: '$DEFAULT_REPLYTO', 
               subject: '$PROJECT_NAME - Build # $BUILD_NUMBER - SUCCESSFUL!', 
               to: '$DEFAULT_RECIPIENTS'
        }
        
        failure {
            emailext body: '$DEFAULT_POSTSEND_SCRIPT', 
               replyTo: '$DEFAULT_REPLYTO', 
               subject: '$PROJECT_NAME - Build # $BUILD_NUMBER - FAILURE!', 
               to: '$DEFAULT_RECIPIENTS'
        }
    }
}
