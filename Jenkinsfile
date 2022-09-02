pipeline {
  agent {
      label 'azure'
  }
  environment{
        VERSION = "${env.BUILD_ID}"
  }

  stages {
      stage('Build and Push Docker Image') {
           steps {
             script{
               withCredentials([string(credentialsId: 'docker_secret', variable: 'docker_secret')]) {
                 dir('store/') {
                   sh '''
                   chmod +x gradlew
                   docker login -u bill3213 -p $docker_secret
                   ./gradlew bootJar -Pprod jib -Djib.to.image=bill3213/store
                   '''
                 }
               }
             }
           }
           steps {
             script{
               withCredentials([string(credentialsId: 'docker_secret', variable: 'docker_secret')]) {
                 dir('invoice/') {
                   sh '''
                   chmod +x gradlew
                   docker login -u bill3213 -p $docker_secret
                   ./gradlew bootJar -Pprod jib -Djib.to.image=bill3213/invoice
                   '''
                 }
               }
             }
           }
           steps {
             script{
               withCredentials([string(credentialsId: 'docker_secret', variable: 'docker_secret')]) {
                 dir('notification/') {
                   sh '''
                   chmod +x gradlew
                   docker login -u bill3213 -p $docker_secret
                   ./gradlew bootJar -Pprod jib -Djib.to.image=bill3213/notification
                   '''
                 }
               }
             }
           }
           steps {
             script{
               withCredentials([string(credentialsId: 'docker_secret', variable: 'docker_secret')]) {
                 dir('product/') {
                   sh '''
                   chmod +x gradlew
                   docker login -u bill3213 -p $docker_secret
                   ./gradlew bootJar -Pprod jib -Djib.to.image=bill3213/product
                   '''
                 }
               }
             }
           }
      }
  }
}

