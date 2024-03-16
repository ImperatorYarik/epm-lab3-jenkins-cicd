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
          sh "docker build -t node${env.BRANCH_NAME}:v1.0. ."
          if (env.BRANCH_NAME == 'main') {
            try {
              container_id=sh "docker ps --filter \"ancestor=nodemain:v.1.0.\" --format \"{{.ID}}\""
              sh "docker rm -f $container_id"
            } catch (Exception e) {
              echo 'No running docker containers, continue pipline'
            }
            sh "docker run -d --expose 3000 -p 3000:3000 nodemain:v1.0."
          } else if (env.BRANCH_NAME == 'dev') {
            try {
              container_id=sh "docker ps --filter \"ancestor=nodemain:v.1.0.\" --format \"{{.ID}}\")"
              sh "docker rm -f $container_id"
            } catch (Exception e) {
              echo 'No running docker containers, continue pipline'
            }
            sh "docker run -d --expose 3001 -p 3001:3000 nodedev:v1.0."
          } else {
            echo "Unknown branch, skipping Docker image build and run"
            return
          }          
          
        }
      }
    }    
  }
}
