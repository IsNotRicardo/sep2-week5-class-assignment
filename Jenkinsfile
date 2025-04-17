pipeline {
    agent any
        environment {
            DOCKERHUB_CREDENTIALS_ID = 'Docker_Hub'
            DOCKERHUB_REPO = 'itsnotricardo/sep2-week5-class-assignment'
            DOCKER_IMAGE_TAG = 'latest_v1'

            SONARQUBE_SERVER = 'SonarQubeServer'
            SONAR_TOKEN = credentials('sonarqube-token')
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
                withSonarQubeEnv('SonarQubeServer') {
                    bat """
                        sonar-scanner ^
                        -Dsonar.projectKey=devops-demo ^
                        -Dsonar.sources=src ^
                        -Dsonar.projectName=DevOps-Demo ^
                        -Dsonar.host.url=http://localhost:9000 ^
                        -Dsonar.java.binaries=target/classes
                    """
                }
            }
        }

        stage('Build Docker Image') {
                    steps {
                        script {
                            docker.build("${DOCKERHUB_REPO}:${DOCKER_IMAGE_TAG}")
                            }
                        }
                    }

        stage('Push Docker Image to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', DOCKERHUB_CREDENTIALS_ID) {
                        docker.image("${DOCKERHUB_REPO}:${DOCKER_IMAGE_TAG}").push()
                    }
                }
            }
        }
    }
}
