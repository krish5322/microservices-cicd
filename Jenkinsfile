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
           sh './kubectl-apply.sh -f'
        }
      }
    }
  }
}

