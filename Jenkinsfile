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
          sh "docker build -t exzenter/node${env.BRANCH_NAME}:v1.0 ."
          
        }
      }
    } 
    stage ('push')  {
      steps {
        withDockerRegistry([ credentialsId: "cee60763-306e-451d-b3ce-d1ae992be316", url: "" ]) {
          sh "docker push exzenter/node${env.BRANCH_NAME}:v1.0"
        }
        
      }
    }
    stage ('trigger deploy') {
      steps {
          build job: "Deploy_to_${env.BRANCH_NAME}", wait: true
      }
    }
    
  }
}
