pipeline {
  agent {
      label 'azure'
  }

  stages {
      stage('Build Artifact') {
           steps {
             sh 'cd store && ./gradlew bootJar -Pprod jib -Djib.to.image=deepu105/store'
           }
      }
  }
}

