pipeline {
  agent {
      label 'azure'
  }
  environment{
        VERSION = "${env.BUILD_ID}"
  }

  stages {
      stage('vulnerability Scan - Docker') {
        steps {
          parallel(
            "Trivy Scan": {
                sh "docker run --rm -v $WORKSPACE:/root/.cache/ aquasec/trivy:0.31.3  image eclipse-temurin:11-jre-focal"
            },
            "OPA Conftest": {
              dir('store/gradle/') {
                sh 'docker run --rm -v $(pwd):/project openpolicyagent/conftest test --policy opa-docker-security.rego docker.gradle'
              }
            }
          )
        }
      }
  }
}

