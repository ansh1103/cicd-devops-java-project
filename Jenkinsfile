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
          sh '''
            docker build -t springapp:${VERSION}
            docker images
          '''
        }        
      }
    }
  }    
}
