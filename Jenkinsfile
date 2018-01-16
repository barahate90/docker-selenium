#!/usr/bin/env groovy
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
         sh "docker -v"
      }
    }
    stage ('Build Chrome Image') {
      steps {
	 sh 'VERSION=${CHROME_VERSION} BUILD_ARGS="--build-arg CHROME_VERSION=${CHROME_VERSION}" make standalone_chrome_debug'
	 echo "chrome with specific verison image build "
	}
  }
  
}
}
