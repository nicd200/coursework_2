pipeline{
    agent any

    stages{
    stage('Build'){
     agent{docker {image 'node:6.3' }}
        steps{

             sh'node server.js'

        }
    }

            stage('Test'){

               environment {
                       scannerHome = tool 'SonarQubeScanner'
                   }
                   steps {

                       withSonarQubeEnv('SonarQube') {


                           sh "${scannerHome}/bin/sonar-scanner -D sonar.login=admin -D sonar.password=admin"
                       }

                   }
            }
            stage('Quality')
            {steps{
            timeout(time: 10, unit: 'MINUTES') {
               waitForQualityGate abortPipeline: true
                                   }
            }
            }
            stage('Image Build')
            {
                steps{
                    script{
                               stage('Build image') {

                                    app = docker.build("nicd200/coursework_2")
                                     docker.withRegistry('https://registry.hub.docker.com', 'docker') {
                                                app.push("${env.BUILD_NUMBER}")
                                                app.push("latest")
                                            }
                                }
                    }
                }
            }

    }
}
