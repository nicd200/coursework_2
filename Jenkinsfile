pipeline {
	agent any

            stage('Sonarqube') {

               environment {
                       scannerHome = tool 'SonarQube'
                   }

                   steps {

                       withSonarQubeEnv('SonarQube') {
                           sh "${scannerHome}/bin/sonar-scanner"
                       }
  
            timeout(time: 10, unit: 'MINUTES') {
               waitForQualityGate abortPipeline: true
                      }
	}	
}

