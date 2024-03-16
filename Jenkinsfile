pipeline {
  agent any

  environment {
        GIT_SSH_COMMAND = 'ssh -o StrictHostKeyChecking=no' // Skip host key checking
    }
  stages {
    stage('checkout') {
      steps {
        echo 'checkout'
      }
    }
    stage('build') { 
      steps { 
        echo 'build'
      }
    }
    stage('test') { 
      steps { 
        echo 'test'
      }
    }
    stage('build docker image') { 
      steps { 
        echo 'build docker image'
      }
    }
  }
}
