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
                "Store-Service-test": {
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
      stage('vulnerability Scan with Trivy - Docker') {
        steps {
          sh "docker run --rm -v $WORKSPACE:/root/.cache/ aquasec/trivy:0.31.3  image eclipse-temurin:11-jre-focal"
        }
      }
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
                 dir('invoice/') {
                   sh '''
                   chmod +x gradlew
                   docker login -u bill3213 -p $docker_secret
                   ./gradlew bootJar -Pprod jib -Djib.to.image=bill3213/invoice
                   '''
                 }
                 dir('notification/') {
                   sh '''
                   chmod +x gradlew
                   docker login -u bill3213 -p $docker_secret
                   ./gradlew bootJar -Pprod jib -Djib.to.image=bill3213/notification
                   '''
                 }
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

      stage('Kubernetes deployment - Dev') {
        steps{
          dir('kubernetes/') {
             sh 'chmod +x kubectl-apply.sh'
             sh './kubectl-apply.sh -f'
          }
        }
      }
      stage('Trivy image scan') {
        steps{
            sh '''
            docker run --rm -v $WORKSPACE:/root/.cache/ aquasec/trivy -q image --exit-code 0 --severity LOW,MEDIUM,HIGH --light /deepu105/invoice-istio
            docker run --rm -v $WORKSPACE:/root/.cache/ aquasec/trivy -q image --exit-code 0 --severity CRITICAL --light /deepu105/notification-istio
            docker run --rm -v $WORKSPACE:/root/.cache/ aquasec/trivy -q image --exit-code 0 --severity LOW,MEDIUM,HIGH --light /deepu105/product-istio
            docker run --rm -v $WORKSPACE:/root/.cache/ aquasec/trivy -q image --exit-code 0 --severity CRITICAL --light /deepu105/store-istio
            '''
        }
      }
      stage('ServiceTest') {
        steps {
            sh '''
            curl -s -o /dev/null -w "%{http_code}" http://store.jhipster.20.204.236.196.nip.io/store
            curl -s -o /dev/null -w "%{http_code}" http://store.jhipster.20.204.236.196.nip.io/product
            curl -s -o /dev/null -w "%{http_code}" http://store.jhipster.20.204.236.196.nip.io/invoice
            curl -s -o /dev/null -w "%{http_code}" http://store.jhipster.20.204.236.196.nip.io/notification
            '''
        }
      }
  }
}

