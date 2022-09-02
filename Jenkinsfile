pipeline {
  agent {
      label 'azure'
  }

  stages {
      stage('Build Artifact') {
           steps {
             sh 'chmod +x gradlew'
             sh 'cd store && ./gradlew bootJar -Pprod jib -Djib.to.image=deepu105/store'
           }
      }
  }
}

