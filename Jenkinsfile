pipeline {
  agent any
  stages {
    stage("Build && Sonarwube analysis {
	  agent {
	    docker {
		  image 'maven'
		}
	  }
	  
	  steps {
	    scripts {
		  withSonarQubeEnv(credentialsId: 'secret-token') {
		    sh 'maven clean package sonar:sonar'
		  }
		}
	  }
	}
  }
}