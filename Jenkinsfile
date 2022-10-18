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
            echo "alias powershell=pwsh" >> /Users/`(whoami)`/.profile . /Users/`(whoami)`/.profile
            pwsh(script: 'docker images -a')
            
         }
      }
   }
   
}
