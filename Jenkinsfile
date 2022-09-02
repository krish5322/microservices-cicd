pipeline {
  agent {
      label 'azure'
  }
  environment{
        VERSION = "${env.BUILD_ID}"
  }

  stages {
      stage('Build Artifact') {
           steps {
             script{
               withCredentials([string(credentialsId: 'docker_secret', variable: 'docker_secret')]) {
                 dir('store/') {
                   sh 'chmod +x gradlew'
                   sh 'docker login -u bill3213 -p $docker_secret'
                   sh './gradlew bootJar -Pprod jib -Djib.to.image=bill3213/microservices-cicd:${VERSION}'
                   sh 'docker rmi bill3213/microservices-cicd:${VERSION}'
                 }
               }
             }
           }
      }
  }
}

