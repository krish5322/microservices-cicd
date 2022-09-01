pipeline {
  agent {
      label 'azure'
  }

  stages {
      stage('Build Artifact') {
            steps {
              dir('invoice/') {
                sh "sudo ./gradlew -Pprod clean bootJar"
                sh 'sudo java -jar build/libs/*.jar'
              }
            }
      }
      stage('Unit test') {
            steps {
              dir('invoice/') {
                sh "sudo ./gradlew test integrationTest jacocoTestReport"
              }
            }

      }
      stage('SonarQube -SAST') {
          steps {
                withSonarQubeEnv('sonar-server2') {
                  dir('invoice/') {
                     sh 'sudo ./gradlew -Pprod clean check jacocoTestReport sonarqube'
                  }
                }
          }
      }
  }
}

