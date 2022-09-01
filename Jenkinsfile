pipeline {
  agent {
      label 'azure'
  }

  stages {
      stage('Build Artifact') {
            steps {
              dir('invoice/') {
                sh 'chmod +x gradlew'
                sh "./gradlew -Pprod clean bootJar"
              }
            }
      }
      stage('Unit test') {
            steps {
              dir('invoice/') {
                sh "./gradlew test integrationTest jacocoTestReport"
              }
            }

      }
      stage('SonarQube -SAST') {
          steps {
                withSonarQubeEnv('sonar-server2') {
                  dir('invoice/') {
                     sh './gradlew -Pprod clean check jacocoTestReport sonarqube'
                  }
                }
          }
      }
  }
}

