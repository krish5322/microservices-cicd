pipeline {
  agent {
      label 'azure'
  }
  environment{
        VERSION = "${env.BUILD_ID}"
  }

  stages {
    dir('invoice/') {
      stage('Build Artifact') {
            steps {
              sh "./gradlew -Pprod clean bootJar"
              sh 'java -jar build/libs/*.jar'
            }
      }
      stage('Unit test') {
            steps {
              sh "./gradlew test integrationTest jacocoTestReport"
            }

      }
      stage('SonarQube -SAST') {
          steps {
                withSonarQubeEnv('sonar-server2') {
                     sh './gradlew -Pprod clean check jacocoTestReport sonarqube'
                }
          }
      }
    }
}

