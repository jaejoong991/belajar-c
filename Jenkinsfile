pipeline {
  agent any

  environment {
    REGISTRY = 'jaejoong991/flask-app'               // Ganti sesuai Docker Hub ID + nama repo
    KUBE = credentials('kubeconfig-id')              // Kubeconfig yang sudah kamu upload di Jenkins
    DOCKER_CREDS = 'jeffgun'                 // Credential ID untuk akses Docker Hub
  }

  stages {
    stage('Checkout') {
      steps {
        git url: 'https://github.com/jaejoong991/belajar-py.git',
            credentialsId: 'github-pat',
            branch: 'main'
      }
    }

    stage('Build Docker') {
      steps {
        script {
          dockerImage = docker.build("${REGISTRY}:${env.BUILD_NUMBER}")
        }
      }
    }

    stage('Push Docker') {
      steps {
        script {
          docker.withRegistry('https://registry.hub.docker.com', DOCKER_CREDS) {
            dockerImage.push()
            dockerImage.push('latest')
          }
        }
      }
    }

    stage('Deploy to K8s') {
      steps {
        withCredentials([file(credentialsId: 'kubeconfig-id', variable: 'KUBECONFIG_FILE')]) {
          sh 'kubectl --kubeconfig "$KUBECONFIG_FILE" apply -f deployment.yaml'
        }
      }
    }

    stage('Smoke Test') {
      steps {
        sh 'sleep 10' // tunggu pod siap
        sh 'kubectl --kubeconfig "$KUBECONFIG_FILE" get svc flask-service -o jsonpath="{.spec.clusterIP}" > ip.txt'
        script {
          def svcIp = readFile('ip.txt').trim()
          sh "curl -f http://${svcIp}:80/ || exit 1"
        }
      }
    }
  }

  post {
    success { echo '🎉 Pipeline selesai dengan sukses!' }
    failure { echo '❌ Ada error—cek logs pipeline!' }
  }
}
