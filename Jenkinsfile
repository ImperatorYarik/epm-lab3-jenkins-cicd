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
                    def imageName = ""
                    if (env.BRANCH_NAME == 'main') {
                        imageName = 'nodemain:v1.0'
                    } else if (env.BRANCH_NAME == 'dev') {
                        imageName = 'nodedev:v1.0'
                    } else {
                        echo "Unknown branch, skipping Docker image build and run"
                        return
                    }
                    
                    try {
                        sh "docker build -t ${imageName} ."
                        sh "docker stop \$(docker ps -q)"
                        sh "docker rm \$(docker ps -aq)"
                        sh "docker run -d --expose 3000 -p 3000:3000 ${imageName}"
                    } catch (Exception e) {
                        echo "Docker image build or container run failed but continuing pipeline execution"
                    }
                }
      }
    }    
  }
}
