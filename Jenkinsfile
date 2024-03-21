pipeline {
  agent any

  environment {
        GIT_SSH_COMMAND = 'ssh -o StrictHostKeyChecking=no' // Skip host key checking
    }
  stages {
    stage('build') { 
      agent { 
        docker {
          image 'node:7.8.0'
          args '-u root:sudo'
          reuseNode true
        }
      }
      steps { 
        sh 'npm install'
      }
    }
    stage('test') { 
      agent {
        docker {
          image 'node:7.8.0'
          args '-u root:sudo'
          reuseNode true
        }
      }
      steps { 
        
        sh 'npm test'
      }
    }
    stage('test Dockerfile by Hadolint') {
      steps {
        sh "docker run --rm -i hadolint/hadolint < Dockerfile"
      }
    }
    stage('Build Docker image') { 
      steps {
        script {
          sh "docker build -t exzenter/node${env.BRANCH_NAME}:v1.0 ."
          
        }
      }
    } 
    stage('Scan Docker image for Vulnerabilities') {
      steps {
        script {
          def vulnerabilities = sh(script: "trivy image --exit-code 0 --scanners vuln --severity HIGH,MEDIUM,LOW --no-progress --cache-dir /tmp/trivy-cache exzenter/node${env.BRANCH_NAME}:v1.0", returnStdout: true).trim()
          echo "vulnerability report:\n${vulnerabilities}"
        }
      }
    }
    stage ('push Docker image to DockerHub')  {
      steps {
        withDockerRegistry([ credentialsId: "cee60763-306e-451d-b3ce-d1ae992be316", url: "" ]) {
          sh "docker push exzenter/node${env.BRANCH_NAME}:v1.0"
        }
        
      }
    }
    stage ('trigger deploy pipeline') {
      steps {
          build job: "Deploy_to_${env.BRANCH_NAME}", wait: true
      }
    }
    
  }
}
