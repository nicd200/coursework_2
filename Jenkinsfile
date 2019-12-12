
pipeline{
environment {
registry = "nicd200/serverjs"
registryCredential = 'dockerhub'
dockerImage = ''
}
    agent any
    stages{
        stage ('build'){
            steps{
                echo 'build'
            }
        }

      stage('Static Analysis') {
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
        stage ('Image Deploy'){
            steps{
             script {
		dockerImage = docker.build registry + ":v1"
		docker.withRegistry( '', registryCredential ) {
		dockerImage.push()
		}
		}
            }
        }
    }
}
