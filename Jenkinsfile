pipeline {
  agent {
      label 'azure'
  }

  stages {
      stage('Build Artifact') {
           steps {
             parallel(
               "invoice-Service-Build": {
                  dir('invoice/') {
                    sh 'chmod +x gradlew'
                    sh "./gradlew -Pprod clean bootJar"
                  }
               },
               "notification-Service-Build": {
                  dir('notification/') {
                    sh 'chmod +x gradlew'
                    sh "./gradlew -Pprod clean bootJar"
                  }
               },
               "Product-Service-Build": {
                  dir('product/') {
                    sh 'chmod +x gradlew'
                    sh "./gradlew -Pprod clean bootJar"
                  }
               },
               "Store-Service-Build": {
                  dir('store/') {
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
                "invoice-Service-test": {
                  dir('invoice/') {
                    sh "./gradlew test integrationTest jacocoTestReport"
                  }
                },
                "notification-Service-test": {
                  dir('notification/') {
                    sh "./gradlew test integrationTest jacocoTestReport"
                  }
                },
                "product-Service-test": {
                  dir('product/') {
                    sh "./gradlew test integrationTest jacocoTestReport"
                  }
                },
                "product-Service-test": {
                  dir('store/') {
                    sh "./gradlew test integrationTest jacocoTestReport"
                  }
                }
              )
            }
      }
      stage('SonarQube -SAST') {
          steps {
              parallel(
                    "Invoice-Service-SAST": {
                       withSonarQubeEnv('sonar-server2') {
                         dir('invoice/') {
                            sh './gradlew -Pprod clean check jacocoTestReport sonarqube'
                         }
                       }
                    },
                    "Notification-Service-SAST": {
                       withSonarQubeEnv('sonar-server2') {
                         dir('notification/') {
                            sh './gradlew -Pprod clean check jacocoTestReport sonarqube'
                         }
                       }
                    },
                    "Product-Service-SAST": {
                       withSonarQubeEnv('sonar-server2') {
                         dir('product/') {
                            sh './gradlew -Pprod clean check jacocoTestReport sonarqube'
                         }
                       }
                    },
                    "Store-Service-SAST": {
                       withSonarQubeEnv('sonar-server2') {
                         dir('store/') {
                            sh './gradlew -Pprod clean check jacocoTestReport sonarqube'
                         }
                       }
                    }
              )
          }
      }
  }

}

