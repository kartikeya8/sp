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


        stage('Snyk Test') {
           steps {
               script {
                   // Run Snyk test
                   withCredentials([string(credentialsId: 'snyk', variable: 'SNYK_TOKEN')]) {
                       sh 'snyk test --token=4b1e47f1-e1b1-4e5a-bdb7-811143cd9466'
                   }
               }
           }
       }
	   }
	   }
