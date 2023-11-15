pipeline {
	agent none
	stages {
		stage('Integration UI Test') {
			parallel {
				stage('Deploy') {
					agent any
					steps {
						bat './jenkins/scripts/deploy.sh'
						input message: 'Finished using the web site? (Click "Proceed" to continue)'
						bat './jenkins/scripts/kill.sh'
					}
				}
				stage('Headless Browser Test') {
					agent any
					steps {
						bat 'mvn -B -DskipTests clean package'
						bat 'mvn test'
					}
					post {
						always {
							junit 'target/surefire-reports/*.xml'
						}
					}
				}
			}
		}
	}
}