pipeline {
  agent any
  tools { 
        maven 'Maven_3_2_5'  
    }
   stages{
    stage('CompileandRunSonarAnalysis') {
            steps {
                  echo '---------------------- Build Started ---------------------------------'      	
		      sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=sgbuggywebapp -Dsonar.organization=sgbuggywebapp -Dsonar.host.url=https://sonarcloud.io -Dsonar.token=36a72ec32a1d519e58cbd95e429f1b7767954d0b'
		      echo '---------------------- Build Completed -------------------------------'	
            }
      }
    
    stage('RunSCAAnalysisUsingSnyk') {
            steps {	
                  echo '---------------------- Snyk Scan Started -----------------------------'  	
				withCredentials([string(credentialsId: 'SNYK_TOKEN', variable: 'SNYK_TOKEN')]) {
					sh 'mvn snyk:test -fn'
				}
                  echo '---------------------- Snyk Scan Completed ---------------------------'  
		}
      }

      stage('Docker Build') { 
            steps { 
                  echo '---------------------- Docker Build Started --------------------------'
                  withDockerRegistry([credentialsId: "dockerlogin", url: ""]) {
                  script {
                        app =  docker.build("asg")
                 }
                 echo '---------------------- Docker Build Completed -------------------------'
               }
            }
      }

      stage('Push') {
            steps {
                  echo '---------------------- Docker Publish Started ------------------------'
                  script {
                    docker.withRegistry('https://471112618192.dkr.ecr.us-east-1.amazonaws.com', 'ecr:us-east-1:aws-credentials') {
                    app.push("latest")
                    }
                  echo '---------------------- Docker Publish Completed ----------------------'
                }
            }
    	}		

  } // end-stages
} // end-pipeline

