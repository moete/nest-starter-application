pipeline {
    environment {
        registry = 'taz98/gearni'
        registryCredential = 'dockerHub'
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
                withSonarQubeEnv(installationName : 'SonarCloudOne' ,credentialsId:'sonarqube-tk' ) { // If you have configured more than one global server connection, you can specify its name
                    sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=gearni-app-key -Dsonar.sources=. -Dsonar.host.url=http://137.184.45.234:9000 -Dsonar.login=d562813b312a9d57179e7ade93a94793395baae0 "
                }
            }
        }

        stage('Building our image') {
            steps {
                script {
                    dockerImage =  docker.build registry + ":$BUILD_NUMBER"
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
