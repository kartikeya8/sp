pipeline {
   agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "Maven 3.6.3"
    }
		
    environment {
        SNYK_TOKEN = credentials('snyk') // Replace 'snyk-api-token' with your actual credential ID
    }

    stages {
	 
	    stage('Code Clone') {
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/kartikeya8/sp.git'
            }
			}
			
        stage('Authenticate with Snyk') {
            steps {
                sh 'snyk auth $SNYK_TOKEN'
            }
        }
        stage('Monitor Project') {
            steps {
                sh '''
                    snyk monitor --org=kartikeya8 --project-id=08219a84-837d-471a-abbe-25b601a0a8f1 --json > report.json
                '''
            }
        }
        stage('Publish Report') {
            steps {
                sh 'cat report.json'
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
		stage('mvn Deploy') {
            steps {
                sh 'mvn deploy'      
            }
			}		         
        }
    
	}


   