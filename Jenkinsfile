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
            sh "/usr/local/bin/pwsh -Command \"Get-Host | Select-Object Version\""
            
            
         }
      }
   }
   
}
