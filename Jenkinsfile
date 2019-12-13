
pipeline{
environment {
registry = "nic6/serverjs"
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
        stage (' Build and Push Image'){
	steps{
             script {
		dockerImage = docker.build registry + ":v2"
		docker.withRegistry( '', registryCredential ) {
		dockerImage.push()
		}
		}
            }
        }
	stage ('Deploy Image'){
	steps {
		sh 'ssh -t azureuser@52.168.52.239 kubectl image deployments/serverjs serverjs=nic6/serverjs:v2'
		}
	}	
    }
}
