pipeline {
    agent any
    tools {
        maven "maven"
    }
	stages {
		stage("Check Out") {
			steps {
				script {
					checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: 'git-cred', url: 'https://github.com/sessiondevops/sonar-app.git']]])
					
				}
			}
		}
		stage("Sonarqube_Chek"){
           steps{
                script{
                    def scannerHome = tool 'sonar';
                    withSonarQubeEnv("sonar"){
			 sh "mvn clean verify sonar:sonar"
                        sh "${scannerHome}/bin/sonar-scanner"
                    }
                }
           }   
        }
        stage("Quality Gate Check"){
            steps{
                script{
                    timeout(time: 3, unit: 'MINUTES'){
                        def qg = waitForQualityGate();
                        if (qg.status != 'OK'){
                            error "Pipleline aborted due to QG failure: ${qg.status}"
                        }
                        
                    }
                }
	    }
            }
	/* stage("Package Development")
        {

            steps

            {

                sh 'mvn clean install'

            }

        
        } */
	}
	post {
        always {
            deleteDir() /* clean up our workspace */
        }
	}	
}
