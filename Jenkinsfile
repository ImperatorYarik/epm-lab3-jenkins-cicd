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
        docker build -t nodemain:v1.0.
        docker rm -f $(docker ps -aq)
        docker run -d --expose 3000 -p 3000:3000 nodemain:v1.0 
      }
    }
  }
}
