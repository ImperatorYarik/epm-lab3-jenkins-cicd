pipeline {
  agent any
  tools {nodejs "7.8.0"}
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
        sh 'docker build -t nodedev:v1.0. .' 
        sh 'docker run -d --expose 3001 -p 3001:3000 nodedev:v1.0.'
      }
    }
  }
}
