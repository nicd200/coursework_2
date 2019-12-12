pipeline{
    agent any

    stages{
    stage('Build'){
     agent{docker {image 'node:6.3' }}
        steps{

             sh'node server.js'

        }
    }

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

