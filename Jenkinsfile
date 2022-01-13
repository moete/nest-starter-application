pipeline {
  agent any
    
  tools {nodejs "node"}
    
  stages {
        
    stage('Cloning Git') {
      steps {
		git branch: 'main', url: 'https://github.com/moete/nest-starter-application.git'      
        }
    }
        
    stage('Install dependencies') {
      steps {
       sh 'npm install'
      }
    }
     
    stage('Test') {
      steps {
         sh 'npm test'
      }
    }
   stage('SonarQube analysis') {
   def scannerHome = tool 'SonarScanner';
        steps{
        withSonarQubeEnv('sonarqube-8.9') { 
        // If you have configured more than one global server connection, you can specify its name
//      sh "${scannerHome}/bin/sonar-scanner"
      sh "${scannerHome}/bin/sonar-scanner"
    }
        }
}