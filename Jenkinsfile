pipeline {
  agent {
      label 'azure'
  }
  environment{
        VERSION = "${env.BUILD_ID}"
  }

  stages {
    steps{
      dir('kubernetes/') {
         sh './kubectl-apply.sh -f'
      }
    }
  }
}

