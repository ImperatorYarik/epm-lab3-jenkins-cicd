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
          def runningContainers = sh(script: 'docker ps -q', returnStdout: true).trim()

                    if (runningContainers) {
                        sh "docker stop ${runningContainers}"
                        sh 'docker rm $(docker ps -aq)'
                    } else {
                        echo "No running containers found. Skipping cleanup."
                    }

        }
        sh 'docker build -t nodemain:v1.0. .'
        sh 'docker run -d --expose 3000 -p 3000:3000 nodemain:v1.0.' 
      }
    }
  }
}
