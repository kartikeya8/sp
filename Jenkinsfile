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
                        snyk monitor --org=kartikeya8 --project-id=08219a84-837d-471a-abbe-25b601a0a8f1 --json > report.json
                    """
                    echo "Snyk monitoring completed successfully."
                }
                
                // Archive report regardless of outcome
                archiveArtifacts artifacts: 'report.json', fingerprint: true
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


	

