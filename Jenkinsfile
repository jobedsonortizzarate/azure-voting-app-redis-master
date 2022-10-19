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
            sh "pwd"
            sh "cd /var/lib/jenkins/workspace/Voting app pipeline/azure-vote;pwd"
            sh "pwd"
            sh "ls"
            sh "docker images -a"
            sh "docker build -t jenkins-pipeline ./azure-vote/."
            sh "docker images -a"
            sh "cd .."
            
         }
      }
   }
   
}
