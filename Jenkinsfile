pipeline {
   agent any

   stages {
      stage('Verify Branch') {
         steps {
            echo "$GIT_BRANCH"
         }
      }

      stage('Docker Build') {
         steps {
            sh "ls"
            sh "cd azure-vote/"
            sh "ls"
            sh "docker images -a"
            sh "docker build -t jenkins-pipeline ./azure-vote/."
            sh "docker images -a"
            sh "cd .."
            
         }
      }
   }
   
}
