pipeline{
    agent{
        label "slave"
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
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/jagannadhamyuvaraj/gitops-pipeline'
            }

        }
        stage("updae the Deployment Tags") {
          steps {
               sh """
                 cat deployment.yaml
                 sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                 cat deployment.yaml
                """
          }
        }
      stage("push the changed deployment file to git") {
        steps {
            // withCredentials([usernamePassword(credentialsId: 'github', passwordVariable: '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', usernameVariable: 'jagannadhamyuvaraj')]) {
          sh """
               git config --global user.name "jagannadhamyuvaraj"
               git config --global user.email "yuvarajjagannadham65@gamil.com"
               git add deployment.yaml
               git commit -m "updated Deployment Manifest"
          """
          withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
            sh "git push https://jagannadhamyuvaraj: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@github.com/jagannadhamyuvaraj/gitops-pipeline.git HEAD:main"
          }
            
        }
      }  
    } //stages closing
} //pipeline closing
