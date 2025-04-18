pipeline {
  agent any

  environment {
    REACT_APP_DIR = 'my-reactjenkinsapp'
    TERRAFORM_DIR = 'terraform'
    AZURE_WEBAPP_NAME = 'webapijenkins691122'
    AZURE_RG = 'rg-jenkins'
    AZURE_PLAN = 'appserviceplanyash'
  }

  stages {
    stage('Install Dependencies') {
      steps {
        dir("${REACT_APP_DIR}") {
          bat 'npm install'
        }
      }
    }

    stage('Build React App') {
      steps {
        dir("${REACT_APP_DIR}") {
          bat 'npm run build'
        }
      }
    }

    stage('Terraform Init') {
      steps {
        dir("${TERRAFORM_DIR}") {
          bat 'terraform init'
        }
      }
    }

    stage('Terraform Apply') {
      steps {
        dir("${TERRAFORM_DIR}") {
          bat 'terraform apply -auto-approve'
        }
      }
    }

    stage('Zip Build Folder') {
      steps {
        dir("${REACT_APP_DIR}\\build") {
          bat 'powershell Compress-Archive -Path * -DestinationPath ..\\build.zip'
        }
      }
    }

    stage('Deploy to Azure App Service') {
      steps {
        dir("${REACT_APP_DIR}") {
          bat """
            az webapp deployment source config-zip ^
              --resource-group %AZURE_RG% ^
              --name %AZURE_WEBAPP_NAME% ^
              --src build.zip
          """
        }
      }
    }
  }

  post {
    success {
      echo "✅ Deployment completed successfully!"
    }
    failure {
      echo "❌ Something went wrong."
    }
  }
}
