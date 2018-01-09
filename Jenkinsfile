pipeline {
  agent any
  stages {
    stage('checkout') {
      steps {
        git 'https://github.com/barahate90/docker-selenium.git'
      }
    }
    stage('VerifyTools') {
      steps {
        sh 'sh "docker -v" '
      }
    }
  }
}