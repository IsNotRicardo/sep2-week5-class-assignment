pipeline {
    agent any

    environment {
        SONARQUBE_SERVER = 'SonarQubeServer'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/IsNotRicardo/sep2-week5-class-assignment.git'
            }
        }

        stage('Build') {
            steps {
                bat 'mvn clean install'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withCredentials([string(credentialsId: 'sonarqube-token', variable: 'SONAR_TOKEN')]) {
                    withSonarQubeEnv('SonarQubeServer') {
                        bat """
                            sonar-scanner ^
                            -Dsonar.projectKey=devops-demo ^
                            -Dsonar.sources=src ^
                            -Dsonar.projectName=DevOps-Demo ^
                            -Dsonar.host.url=http://localhost:9000 ^
                            -Dsonar.login=${SONAR_TOKEN} ^
                            -Dsonar.java.binaries=target/classes
                        """
                    }
                }
            }
        }
    }
}
