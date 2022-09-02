pipeline {
  agent {
      label 'azure'
  }
  environment{
        VERSION = "${env.BUILD_ID}"
  }

  stages {
    stage('Kubernetes deployment - Dev') {
      steps{
        dir('kubernetes/') {
           sh 'chmod +x kubectl-apply.sh'
           sh './kubectl-apply.sh -f'
        }
      }
    }
  }
}

