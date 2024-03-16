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
      when {
        branch 'main'
      }
      steps {
        script {
          try {
            sh 'docker stop $(docker ps -q)'
            sh 'docker rm $(docker ps -aq)'
          } catch (Exception e) {
            echo "There are no running containers"
          }
          
        } 
        sh 'docker build -t nodemain:v1.0. .'
        sh 'docker run -d --expose 3000 -p 3000:3000 nodemain:v1.0.' 
      }
    }
    stage('build docker image') { 
      when {
        branch 'dev'
      }
      steps {
        script {
          try {
            sh 'docker stop $(docker ps -q)'
            sh 'docker rm $(docker ps -aq)'
          } catch (Exception e) {
            echo "There are no running containers"
          }
        } 
        sh 'docker build -t nodedev:v1.0. .'
        sh 'docker run -d --expose 3001 -p 3001:3000 nodedev:v1.0.' 
      }
    }
  }
}
