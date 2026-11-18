pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        sh '''docker --version
docker build -t mini .'''
      }
    }

    stage('Run Docker Container') {
      steps {
        sh '''docker rm -f mini1 >/dev/null 2>&1 || true
docker run -d -p 8081:80 --name mini1 mini'''
      }
    }

    stage('verify Docker Container') {
      steps {
        sh 'docker ps'
      }
    }

  }
}