pipeline {
  agent any

  environment {
        GIT_SSH_COMMAND = 'ssh -o StrictHostKeyChecking=no' // Skip host key checking
    }
  stages {
    stage('build') { 
      steps { 
        sh 'npm install'
      }
    }
    stage('test') { 
      steps { 
        sh 'npm test'
      }
    }
    stage('build docker image') { 
      steps { 
        sh 'docker build -t nodemain:v1.0. .'
        sh 'docker rm -f $(docker ps -aq)'
        sh 'docker run -d --expose 3000 -p 3000:3000 nodemain:v1.0' 
      }
    }
  }
}
