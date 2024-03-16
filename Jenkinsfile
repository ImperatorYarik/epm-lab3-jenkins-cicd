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
        script {
          if [ "$(docker ps -q)" ]; then
              docker stop $(docker ps -q)              
              docker rm $(docker ps -aq)
          fi
        }
        sh 'docker build -t nodemain:v1.0. .'
        sh 'docker run -d --expose 3000 -p 3000:3000 nodemain:v1.0.' 
      }
    }
  }
}
