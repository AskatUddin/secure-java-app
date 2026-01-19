pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "askat777/springboot-app:latest"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/AskatUddin/secure-java-app.git'

            }
        }

        stage('Build') {
            steps {
                // Use Maven wrapper
                sh './mvnw clean install -DskipTests=false'
            }
        }

        stage('Unit Test') {
            steps {
                sh './mvnw test'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh 'echo $PASSWORD | docker login -u $USERNAME --password-stdin'
                    sh 'docker push $DOCKER_IMAGE'
                }
            }
        }
    }
}
