node {
   stage('Build') {
      sh "./gradlew clean build"
   }
   stage('Results') {
      junit '**/test-results/test/TEST-*.xml'
      emailext body: '$DEFAULT_CONTENT', replyTo: '$DEFAULT_REPLYTO', subject: '$DEFAULT_SUBJECT', to: '$DEFAULT_RECIPIENTS'        
   }
}
