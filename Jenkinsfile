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
	stage ('Build Firefox Image') {
      steps {
		sh 'VERSION=${FIREFOX_VERSION} BUILD_ARGS="--build-arg FIREFOX_VERSION=${FIREFOX_VERSION}" make standalone_firefox_debug'
		echo "chrome with specific verison image build "
	}
  }
	stage('docker-compose'){
	cat > docker-compose.yml <<EOF
	hub+$BRANCH_NAME+$BUILD_NUMBER:
		image:selenium/hub:${GRID_VERSION}
		restart: always
		ports:
		- "4444:4444"
	chrome+$BRANCH_NAME+$BUILD_NUMBER:
		image:selenium/node-chrome:${CHROME_VERSION}
		restart: always
		hostname: chrome+$BRANCH_NAME+$BUILD_NUMBER
		ports:
			- '5944:5900'
		links:
			- hub
		volumes:
			- /dev/shm:/dev/shm
	firefox$BRANCH_NAME+$BUILD_NUMBER:
		image: selenium/node-firefox-debug:${FIREFOX_VERSION}
		restart: always
		hostname: firefox$BRANCH_NAME+$BUILD_NUMBER
		ports:
			- '5943:5900'
		links:
			- hub
		volumes:
			- /dev/shm:/dev/shm 
	EOF
	}
}
}
