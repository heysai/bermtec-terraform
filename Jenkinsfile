pipeline {
  agent any

  parameters {
    password (name: 'AWS_ACCESS_KEY_ID')
    password (name: 'AWS_SECRET_ACCESS_KEY')
  }

  environment {
    varfile= "${params.file}"
    AWS_ACCESS_KEY_ID = "${params.AWS_ACCESS_KEY_ID}"
    AWS_SECRET_ACCESS_KEY = "${params.AWS_SECRET_ACCESS_KEY}"
  }

  stages {
    stage('Init') {
      steps {
        sh "terraform init"
        echo 'Initialization done'
      }
    }

    stage('plan') {
      steps {
        sh "terraform plan -var-file='${varfile}'"
        echo 'plan done'
      }
    }

    stage('apply') {
      steps {
        script {
          def xy = input(message:'Apply ?', ok: 'Yes') 
          echo "Text: " + xy
          sh "terraform apply -var-file='${varfile}' --auto-approve"
          echo 'Apply done'
        }
      }
    }

     stage('destroy') {
      steps {
        input 'Destroy ?'
        sh "terraform destroy -var-file='${varfile}' --auto-approve"
        echo 'Destroy done'
      }
    }
  }
}