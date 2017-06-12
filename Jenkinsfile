node {
   stage('Checkout') {
      emailext body: '$DEFAULT_CONTENT', replyTo: '$DEFAULT_REPLYTO', subject: '$DEFAULT_SUBJECT', to: '$DEFAULT_RECIPIENTS'
      checkout scm
   }
   stage('Build') {
      sh "ls -la"
      sh "./gradlew clean build"
   }
   stage('Results') {
      junit '**/test-results/test/TEST-*.xml'       
   }
   stage('Done') {
        emailext body: '$DEFAULT_CONTENT', replyTo: '$DEFAULT_REPLYTO', subject: '$DEFAULT_SUBJECT', to: '$DEFAULT_RECIPIENTS'       
   }
}
