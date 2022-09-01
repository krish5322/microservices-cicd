pipeline {
  agent {
      label 'azure'
  }

  stages {
      stage('Build Artifact') {
            steps {
              parallel(
               "invoiceServiceBuild": {
                  dir('invoice/') {
                    sh 'chmod +x gradlew'
                    sh "./gradlew -Pprod clean bootJar"
                  }
               },
               "notificationBuild": {
                  dir('notification') {
                    sh 'chmod +x gradlew'
                    sh "./gradlew -Pprod clean bootJar"
                  }
               }
              )
            }
      }
      stage('Unit test') {
            steps {
              parallel(
                "invoicetest": {
                  dir('invoice/') {
                    sh "./gradlew test integrationTest jacocoTestReport"
                  }
                },
                "notificationtest": {
                  dir('notification/') {
                    sh "./gradlew test integrationTest jacocoTestReport"
                  }
                }
              )
            }
      }
      stage('SonarQube -SAST') {
          steps {
              parallel(
                    "invoiceSAST": {
                       withSonarQubeEnv('sonar-server2') {
                         dir('invoice/') {
                            sh './gradlew -Pprod clean check jacocoTestReport sonarqube'
                         }
                       }
                    },
                    "notificationSAST": {
                       withSonarQubeEnv('sonar-server2') {
                         dir('notification/') {
                            sh './gradlew -Pprod clean check jacocoTestReport sonarqube'
                         }
                       }
                    }
              )
          }
      }
  }

}

