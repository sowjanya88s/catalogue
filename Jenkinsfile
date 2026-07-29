pipeline {
    agent {
	node {
		label 'roboshop'
	     }
	  }
      environment {
        appVersion = ""
      }
      options {
        timeout(time: 5, unit: 'MINUTES')
      }
    stages {
        stage('read version') {
            steps {
               script {
		            
                     // Read the package.json file into an object
                    def packageJson = readJSON file: 'package.json'
                    
                    // Extract the version field
                     appVersion = packageJson.version
                    
                    // Print or use the variable in subsequent steps
                    echo "The application version is: ${appVersion}"
            
   		}
            }
        }
        stage('install dependancies') {
            steps {
                script {
		          sh """
                    npm install
                  """
   		}
            }
        }
        stage('build') {
            steps {
                script {
		          sh """
                    docker build -t catalogue:${appVersion} .
                """
   		}
            }
        }
    }
    
post {
	always {
	  echo 'i will say hello again!'
		}
	success {
	  echo 'pipeline success'
		}
	failure {
	  echo 'pipeline failure'
		}
	}
}
