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
         environment {
            WEB_IMAGE_NAME="${ACR_LOGINSERVER}/siaraf/azure-vote-front:kube${BUILD_NUMBER}"
         }
         steps {
            echo "Workspace is $WORKSPACE"
            dir("$WORKSPACE/azure-vote")
            {
               script
               {
                  docker.withRegistry('https://acrindep.azurecr.io', 'acr-credentials')
                  {
                     def image = docker.build("$WEB_IMAGE_NAME")
                     image.push()
                  }
               }
            }
         }
      }

      // stage('Push container') {
      //    environment {
      //       WEB_IMAGE_NAME="${ACR_LOGINSERVER}/siaraf/azure-vote-front:kube${BUILD_NUMBER}"
      //    }
      //    steps {
      //       echo "ACR is ${ACR_LOGINSERVER}"
      //       sh(script: """
      //       # Build new image and push to ACR.
            
      //       docker build -t $WEB_IMAGE_NAME ./azure-vote
      //       docker login ${ACR_LOGINSERVER} -u ${ACR_ID} -p ${ACR_PASSWORD}
      //       docker push $WEB_IMAGE_NAME
      //          """)
      //    }
      // }

      stage('Deploying container.') {
         environment {
            ENVIRONMENT = 'qa'
            WEB_IMAGE_NAME="${ACR_LOGINSERVER}/siaraf/azure-vote-front:kube${BUILD_NUMBER}"
         }
         steps {
            echo "Deploying to ${ENVIRONMENT}"
         
            sh(script: """
            # Update kubernetes deployment with new image.
            kubectl set image deployment/azure-vote-front azure-vote-front="$WEB_IMAGE_NAME"
            """)

            // acsDeploy(
            //    azureCredentialsId: "jenkins_demo",
            //    configFilePaths: "**/*.yaml",
            //    containerService: "${ENVIRONMENT}-demo-cluster | AKS",
            //    resourceGroupName: "${ENVIRONMENT}-demo",
            //    sshCredentialsId: ""
            // )
         }
      }

   }
   
}
