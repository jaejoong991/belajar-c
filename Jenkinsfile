pipeline {
  agent any

  environment {
    REG = 'jaejoong991/flask-app'
    KUBECONFIG = credentials('kubeconfig-id')
  }

  stages {
    stage('Build & Push') {
      steps {
        sh 'echo building...'
        // build dan push docker
      }
    }
    stage('Deploy') {
      steps {
        sh 'echo deploying...'
        // kubectl apply
      }
    }
  }

  post {
    always {
      echo 'Done'
    }
  }
}
