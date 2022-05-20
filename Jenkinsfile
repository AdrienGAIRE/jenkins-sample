// Powered by Infostretch 

timestamps {

node () {

	stage ('App-IC - Checkout') {
 	 checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'git-login', url: 'https://github.com/AdrienGAIRE/jenkins-sample.git']]]) 
	}
	
	stage ('App-IC - Compile') {
	withMaven(maven: 'maven') { 
 			if(isUnix()) {
 				sh "mvn clean compile " 
			} else { 
 				bat "mvn clean compile " 
			} 
 		} 
	}
	
	stage ('App-IC - Test') {
	withMaven(maven: 'maven') { 
 			if(isUnix()) {
 				sh "mvn test" 
			} else { 
 				bat "mvn test" 
			} 
 		} 
	}
	
	stage ('App-IC - package') {
	withMaven(maven: 'maven') { 
 			if(isUnix()) {
 				sh "mvn package -DskipTests" 
			} else { 
 				bat "mvn package -DskipTests" 
			} 
 		} 
	}
	
	stage('Quality check') {
		withSonarQubeEnv('Sonar') {
			if(isUnix()) {
 				sh "mvn verify org.sonarsource.scanner.maven:sonar-maven-plugin:sonar -Dsonar.projectKey=Jenkins-demo"
			} else { 
				bat "mvn verify org.sonarsource.scanner.maven:sonar-maven-plugin:sonar -Dsonar.projectKey=Jenkins-demo"
			} 
		}
	} 
	stage ('App-IC - Post build actions') {
/*
Please note this is a direct conversion of post-build actions. 
It may not necessarily work/behave in the same way as post-build actions work.
A logic review is suggested.
*/
		// Mailer notification
		step([$class: 'Mailer', notifyEveryUnstableBuild: true, recipients: 'adrien.gaire@gmail.com', sendToIndividuals: false])
 
	}
}
}
