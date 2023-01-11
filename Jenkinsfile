pipeline {
  agent any
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
  }    
}
