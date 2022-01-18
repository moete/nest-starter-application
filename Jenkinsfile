pipeline {
    environment {
        registry = 'taz98/gearni'
        registryCredential = 'taz98'
        dockerImage = ''
    }
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
        stage('SonarQube analysis') {
            environment {
                scannerHome = tool 'SonarQubeScanner'
                organization = ''
                project_name = ''
            }
            steps {
                //, credentialsId:'sonarqube-token'
                withSonarQubeEnv(installationName : 'SonarCloudOne' ) { // If you have configured more than one global server connection, you can specify its name
                    sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=gearni-app -Dsonar.sources=. "
                }
            }
        }

        stage('Building our image') {
            steps {
                script {
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                }
            }
        }
        stage('Deploy our image') {
            steps {
                script {
                    docker.withRegistry( '', registryCredential ) {
                        dockerImage.push()
                    }
                }
            }
        }
        stage('Cleaning up') {
            steps {
                sh "docker rmi $registry:$BUILD_NUMBER"
            }
        }
    }
}
