pipeline {
  agent any
  environment {
    VERSION = "${env.BUILD_ID}"
  }
  stages {
    stage("Build && Sonarqube analysis") {
      agent {
        docker { image 'maven'
        }
      }
      steps {
        script {
          withSonarQubeEnv(credentialsId: 'secret-token') {
            sh 'mvn clean package sonar:sonar'
          }
        }
      }
    }
    stage("Quality Gate Check") {
      steps {
        script {
          waitForQualityGate abortPipeline: false, credentialsId: 'secret-token'
        }
      }
    }
    stage("Docker build && push image to Nexus repo") {
      steps {
        script {
          withCredentials([string(credentialsId: 'nexus-secret', variable: 'token-nexus')]) {
            sh '''
            docker build -t 65.2.182.14:8083/springapp:${VERSION} .
            docker login -u admin -p $token-nexus 65.2.182.14:8083
            docker push 65.2.182.14:8083/springapp:${VERSION}
            docker rmi 65.2.182.14:8083/springapp:${VERSION}
            '''
          }
        }        
      }
    }
  }    
}
