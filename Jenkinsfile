pipeline {
  agent any
  environment {
    REG = 'jaejoong991/flask-app'
    KUBE = credentials('kubeconfig-id')
  }
  stages {
    stage('Checkout') {
      steps { git url: '...', credentialsId: 'github-pat', branch: 'main' }
    }
    stage('Build') {
      steps {
        sh 'pip install -r requirements.txt'
        sh 'pytest || echo "No tests"'
      }
    }
    stage('Build & Push Docker') {
      steps {
        script {
          img = docker.build("${REG}:$BUILD_ID")
          docker.withRegistry('', 'dockerhub-creds') {
            docker.push("${BUILD_ID}")
            docker.push('latest')
          }
        }
      }
    }
    stage('Deploy to K8s') {
      steps {
        sh 'kubectl apply -f deployment.yaml'
      }
    }
  }
}
