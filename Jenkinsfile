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
   def scannerHome = tool 'SonarScanner 4.0';
        steps{
        withSonarQubeEnv('sonarqube-8.9') { 
      sh "${scannerHome}/bin/sonar-scanner"
    }    
 }
}
  }
}