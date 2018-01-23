#!/usr/bin/env groovy
pipeline {
  agent { node {
	  label 'docker-host'
  	  customWorkspace '/home/akmba5/CBT/jenkins/workspace/docker-selenium'	
  }
	}
  stages {
    stage('VerifyTools') {
      steps {
         sh  '''
	      docker -v
	      docker-compose -v
	      '''
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
   stage('dynamic-docker-compose'){
	 steps {	
	sh '''
		cat > docker-compose.yml <<EOF
hub:
  image: 'selenium/hub:${GRID_VERSION}'
  restart: always
  ports:
      - '${HUB_PORT}:4444'
chrome:
  image: 'selenium/node-chrome:${CHROME_VERSION}'
  restart: always
  hostname: chrome+$BRANCH_NAME+$BUILD_NUMBER
  ports:
      - '5950:5900'
  links:
      - hub
  volumes:
      - '/dev/shm:/dev/shm'
firefox:
  image: 'selenium/node-firefox-debug:${FIREFOX_VERSION}'
  restart: always
  hostname: firefox+$BRANCH_NAME+$BUILD_NUMBER
  ports:
      - '5951:5900'
  links:
      - hub
  volumes:
      - '/dev/shm:/dev/shm' 
EOF
	
	'''
		echo 'not using shell'
	}	
	}
stage('docker-compose-running'){
	steps {	
		sh "docker-compose up -d"
		echo "provisioning the docker-compose environment"
	}
	
}
	  stage('report-generator'){
		  steps{
		sh '''
		cat > grid_info.json <<EOF
{
"GRID_URL":"http://10.239.34.90:${HUB_PORT}/grid/console",
"GRID_VERSION" :"${GRID_VERSION}",
"CHROME_VERSION":"${CHROME_VERSION}",
"FIREFOX_VERSION":"${FIREFOX_VERSION}"
}
		EOF
	
	'''
	  }
}
}
}
