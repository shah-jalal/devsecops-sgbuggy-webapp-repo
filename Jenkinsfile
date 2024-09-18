pipeline {
  agent any
  tools { 
        maven 'Maven_3_2_5'  
    }
   stages{
    stage('CompileandRunSonarAnalysis') {
            steps {	
		sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=sgbuggywebapp -Dsonar.organization=sgbuggywebapp -Dsonar.host.url=https://sonarcloud.io -Dsonar.token=36a72ec32a1d519e58cbd95e429f1b7767954d0b'
			}
        }
    
    stage('RunSCAAnalysisUsingSnyk') {
            steps {		
				withCredentials([string(credentialsId: 'SNYK_TOKEN', variable: 'SNYK_TOKEN')]) {
					sh 'mvn snyk:test -fn'
				}
			}
    }		

  } // stages
} // pipeline
