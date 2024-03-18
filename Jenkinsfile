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
    stage('docker build') { 
      steps {
        script {
          
          sh "docker build -t node${env.BRANCH_NAME}:v1.0. ."
               
          
        }
      }
    } 
    stage ('deploy')  {
      steps {
        script {
          
          if (env.BRANCH_NAME == 'main') {
            try {
              ENV_CONTAINER_ID = sh (
                  script: 'docker ps -aqf "ancestor=nodemain:v1.0."',
                  returnStdout: true
              ).trim()  
              sh "docker rm -f ${ENV_CONTAINER_ID}"
          } catch (Exception e) {
              echo 'No running docker containers, continue pipline'
          }
            sh "docker run -d --expose 3000 -p 3000:3000 nodemain:v1.0."
          } else if (env.BRANCH_NAME == 'dev') {
            try {   
              ENV_CONTAINER_ID = sh (
                  script: 'docker ps -aqf "ancestor=nodedev:v1.0."',
                  returnStatus: true
              ) == 0    
              sh "docker rm -f ${ENV_CONTAINER_ID}" 
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
