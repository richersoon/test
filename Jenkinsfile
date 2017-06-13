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
        always {
            echo 'One way or another, I have finished'
            deleteDir() /* clean up our workspace */
        }
        success {
            echo 'I succeeeded!'
        }
        unstable {
            echo 'I am unstable :/'
        }
        failure {
            echo 'I failed :('
        }
        changed {
            echo 'Things were different before...'
        }
    }
}
