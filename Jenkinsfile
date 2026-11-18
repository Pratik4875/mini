pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        sh '''docker --version
docker build -t mini .'''
      }
    }

  }
}