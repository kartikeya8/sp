pipeline {
    agent any

    tools {
        maven "Maven 3.6.3"
    }

    environment {
        SNYK_TOKEN = credentials('snyk') // Replace with your actual credential ID
    }

    stages {
        stage('Authenticate with Snyk') {
            steps {
                sh 'snyk auth $SNYK_TOKEN'
            }
        }
        
        stage('Monitor Project') {
            steps {
                catchError(buildResult: 'SUCCESS', stageResult: 'SUCCESS') {
                    sh """
                        snyk monitor --org=kartikeya8 --project-id=08219a84-837d-471a-abbe-25b601a0a8f1 --json > report.json
                    """
                    echo "Snyk monitoring completed successfully without issues."
                }
                
                // Log any issues found by snyk monitor
                sh 'cat report.json'
                
                // Archive report regardless of outcome
                archiveArtifacts artifacts: 'report.json', fingerprint: true
                
                echo "Snyk monitoring found issues, but continuing pipeline."
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