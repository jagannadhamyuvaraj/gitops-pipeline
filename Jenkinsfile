pipeline{
    agent{
        label "slave"
    }
  tools {
        maven 'Maven3'
     
        // sonarqube 'sonarqube-scanner'
    }
  environment {
        
        APP_NAME = "complete-prodcution-e2e-pipeline"
    }
  
  
    stages{
        stage("Cleanup Workspace"){
            steps {
                cleanWs()
            }

        }
    
        stage("Checkout from SCM"){
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/jagannadhamyuvaraj/gitops-pipeline.git'
            }

        }
        stage("updae the Deployment Tags") {
          steps {
                 """
                 cat deployment.yaml
                 sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deloyment.yaml
                 cat deployment.yaml
                 """
          }
        }
      stage("push the changed deployment file to git") {
        steps {
          sh """
               git config --global user.name "jagannadhamyuvaraj"
               git config --global user.email "yuvarajjagannadham65@gamil.com"
               git add deployment.yaml
               git commit -m "updated Deployment Manifest"
          """
          withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
            sh "'git push https://github.com/jagannadhamyuvaraj/gitops-pipeline main"
          }
        }
      }  
    } //stages closing
} //pipeline closing

    }
