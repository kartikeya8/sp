pipeline {
   agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "Maven 3.6.3"
    }

    stages {
        stage('Code Clone') {
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/kartikeya8/sp.git'
            }
			}			


			
		stage('Monitor Project') {
            steps {
                catchError(buildResult: 'SUCCESS', stageResult: 'SUCCESS') {
                    sh """
					    chmod +x ./mvnw
					    snyk auth 4b1e47f1-e1b1-4e5a-bdb7-811143cd9466
						snyk code test
						snyk test --json --severity-threshold=low
                        snyk monitor --org=kartikeya8 --project-id=08219a84-837d-471a-abbe-25b601a0a8f1 --json > report.json
                    """
                    echo "Snyk monitoring completed successfully."
                }
                
                // Archive report regardless of outcome.
                archiveArtifacts artifacts: 'report.json', fingerprint: true
            }
        }
        
		stage('SonarScan') {
            steps {
                sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=kspro -Dsonar.host.url=http://192.168.29.30:9000 -Dsonar.login=sqp_727cd2c07493015efb63de36da645a0faaf5ae01'      
            }
			}
	
		stage('mvn version') {
            steps {
              sh 'mvn --version'    
            }
			}	
		stage('mvn clean') {
            steps {
                sh 'mvn clean'      
            }
			}
		stage('mvn validate') {
            steps {
                sh 'mvn validate'      
            }
			}
		stage('mvn Compile') {
            steps {
                sh 'mvn compile'      
            }
			}
		stage('mvn Test') {
            steps {
                sh 'mvn test'      
            }
			}
	
        			
		stage('mvn Package') {
            steps {
                sh 'mvn package'      
            }
			}
	
	
	
	
	}

    post {
        success {
            echo 'Pipeline completed successfully.'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}



	

