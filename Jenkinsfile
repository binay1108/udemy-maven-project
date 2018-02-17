pipeline {
	agent any

	parameters {
			 string(name: 'tomcat_dev', defaultValue: '54.154.2.62', description: 'Staging Server')
			 string(name: 'tomcat_prod', defaultValue: '54.154.233.5', description: 'Production Server')
		   }

	triggers {
		 pollSCM('* * * * *')
		 }

	
	stages{
	        stage('Build') {
			         steps {
				         sh 'mvn clean package'
				       }
				 post {
					success {
						echo 'Now Archiving...'
						archiveArtifacts artifacts: '**/target/*.war'
						}
				      }
				}

		stage ('Deployments') {
		   parallel {
		     stage ('Deploy to Staging'){
			steps {
		 	  sh "scp -i /var/lib/jenkins/.ssh/MyEc2KeyPair.pem **/target/*.war ubuntu@${params.tomcat_dev}:/opt/tomcat/webapps"
			}
		     }

		     stage ("Deploy to Production"){
		       steps {
			  sh "scp -i /var/lib/jenkins/.ssh/MyEc2KeyPair.pem **/target/*.war ubuntu@${params.tomcat_prod}:/opt/tomcat/webapps/."
		       }
		     }
		   }
		}
            }
}
