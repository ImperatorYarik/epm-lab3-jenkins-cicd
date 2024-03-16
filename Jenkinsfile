pipeline {
  agent any

  environment {
        GIT_SSH_COMMAND = 'ssh -o StrictHostKeyChecking=no' // Skip host key checking
    }
  stages {
    stage('build') { 
      steps { 
        npm install
      }
    }
    stage('test') { 
      steps { 
        npm test
      }
    }
    stage('build docker image') { 
      steps { 
        docker build -t "nodedev:v1.0." . 
        docker rm -f $(docker ps -aq)
        docker run -d --expose 3001 -p 3001:3000 "nodedev:v1.0."
      }
    }
  }
}
