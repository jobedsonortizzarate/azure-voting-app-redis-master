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
            sh "docker images -a"
            sh "docker build -t jenkins-pipeline ./azure-vote/."
            sh "docker images -a"
            sh "cd .."
            
         }
      }
   }
   
}
