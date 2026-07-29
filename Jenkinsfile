pipeline {
    agent {
	node {
		label 'roboshop'
	     }
	  }
      environment {
        appVersion = ""
        ACC_ID = "357323381515"
        region = "us-east-1"
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
              withAWS(credentials: 'aws-auth', region: "${region}")
                script {
		          sh """
                   aws ecr get-login-password --region ${region} | docker login --username AWS --password-stdin ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com
                   docker build -t  ${ACC_ID}.dkr.ecr.${region}.amazonaws.com/roboshop/catalogue:${appVersion} .
                   docker push ${ACC_ID}.dkr.ecr.${region}.amazonaws.com/roboshop/catalogue:${appVersion}
                    
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
