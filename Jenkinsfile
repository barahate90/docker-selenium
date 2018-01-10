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
     stage ('Build app') {
      steps {
       sh  "docker ps"
	    }
    }
  }
}
