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
            
         }
      }

      stage('Start test app') {
         steps {
            sh(script: """
               docker-compose up -d
               scripts/test_container.sh
            """)
         }
         post {
            success {
               echo "App started successfully :)"
            }
            failure {
               echo "App failed to start :("
            }
         }
      }
   }
   
}
