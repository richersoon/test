pipeline {
   agent any
   stages {
      stage('Checkout') {
         checkout scm
      }
      stage('Build') {
         sh "ls -la"
         sh "./gradlew clean build"
      }
      stage('Results') {
         junit '**/test-results/test/TEST-*.xml'       
      }
   }
   
   post {
        success {
            
        }
        
        failure {
         
        }
   }
}
