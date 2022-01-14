pipeline {
    agent any

    tools { nodejs 'node' }

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
        stage('SonarQube Analysis') {
            def scannerHome = tool 'sonarqubescanner'
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh "${scannerHome}/bin/sonarqubescanner"
                }
            }
        }
    }
}
