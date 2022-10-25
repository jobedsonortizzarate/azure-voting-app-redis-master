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
               chmod +x -R ${env.WORKSPACE}
               ./scripts/test_container.sh
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

      stage('Run Tests') {
         steps {
            sh(script: """
               pytest ./tests/test_sample.py
            """)
         }
      }

      stage('Stop test app') {
         steps {
            sh(script: """
               docker-compose down
            """)
         }
      }

      stage('Push container') {
         steps {
            echo "Workspace is $WORKSPACE"
            dir("$WORKSPACE/azure-vote")
            {
               script
               {
                  docker.withRegistry('https://index.docker.io/v1/', 'DockerHub')
                  {
                     def image = docker.build('jobedson/demokenkins:latest')
                     image.push()
                  }
               }
            }
         }
      }

      stage('Deploy to QA') {
         environment {
            ENVIRONMENT = 'qa'
         }
         steps {
            echo "Deploying to ${ENVIRONMENT}"
            acsDeploy(
               azureCredentialsId: "jenkins_demo",
               configFilePaths: "**/*.yaml",
               containerService: "${ENVIRONMENT}-demo-cluster | AKS",
               resourceGroupName: "${ENVIRONMENT}-demo",
               sshCredentialsId: ""
            )
         }
      }

   }
   
}
