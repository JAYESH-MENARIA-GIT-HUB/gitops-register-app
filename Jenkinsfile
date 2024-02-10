pipeline {
    agent { label "Jenkins-Agent" }
    environment {
            APP_NAME = "register-app-pipeline"
            RELEASE = "1.0.0"
            IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"
    }

    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Checkout from SCM") {
               steps {
                   git branch: 'main', credentialsId: 'github', url: 'https://github.com/JAYESH-MENARIA-GIT-HUB/register-app'
               }
        }

        stage("Update the Deployment Tags") {
            steps {
                sh """
                   cat gitops-register-app/deployment.yaml
                   sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' gitops-register-app/deployment.yaml
                   cat gitops-register-app/deployment.yaml
                """
            }
        }

        stage("Push the changed deployment file to Git") {
            steps {
                sh """
                   git config --global user.name "jayesh menaria"
                   git config --global user.email "menariaj3@gmail.com"
                   git add deployment.yaml
                   git commit -m "Updated Deployment Manifest"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                  sh "git push https://github.com/JAYESH-MENARIA-GIT-HUB/gitops-register-app main"
                }
            }
        }
      
    }
}
