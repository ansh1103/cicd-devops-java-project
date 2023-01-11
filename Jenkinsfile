pipeline {
  agent any
  stages {
    stage("Build && Sonarwube analysis") {
      agent {
        any { image 'maven' }}
        steps {
	  script {
	    withSonarQubeEnv(credentialsId: 'secret-token') {
	      sh 'maven clean package sonar:sonar'
	    }
	  }
        }
      }
  }
}
