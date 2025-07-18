pipeline {
  agent {
    kubernetes {
      label 'gcc'           // harus match label Pod Template agent
      defaultContainer 'gcc'
      yaml """
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: gcc
    image: gcc:13
    command:
    - cat
    tty: true
"""
    }
  }
  stages {
    stage('Clone') {
      steps { checkout scm }
    }
    stage('Build & Run C') {
      steps {
        container('gcc') {
          sh 'gcc hello.c -o hello'
          sh './hello'
        }
      }
    }
  }
}
