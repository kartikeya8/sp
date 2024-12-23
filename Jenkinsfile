pipeline {
    agent any

    tools {
        maven "Maven 3.6.3"
    }

    environment {
        SNYK_TOKEN = credentials('snyk')
    }

    stages {
        stage('Authenticate with Snyk') {
            steps {
                sh 'snyk auth $SNYK_TOKEN'
            }
        }
        
        stage('Monitor Project') {
            steps {
                script {
                    def exitCode = sh(
                        script: """
                            snyk monitor --org=kartikeya8 --project-id=08219a84-837d-471a-abbe-25b601a0a8f1 --json > report.json
                        """,
                        returnStatus: true
                    )
                    
                    if (exitCode != 0) {
                        echo "Snyk monitoring found issues, but continuing pipeline."
                    } else {
                        echo "Snyk monitoring completed successfully without issues."
                    }
                    
                    // Archive report regardless of outcome
                    archiveArtifacts artifacts: 'report.json', fingerprint: true
                }
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