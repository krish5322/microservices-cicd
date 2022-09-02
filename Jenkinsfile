pipeline {
  agent {
      label 'azure'
  }

  stages {
      stage('Build Artifact') {
           steps {
             dir('store/') {
               sh 'chmod +x gradlew'
               sh './gradlew bootJar -Pprod jib -Djib.to.image=deepu105/store'
             }
           }
      }
  }
}

