pipeline {
    agent any

    environment {
        registry = “andrespspano/desafio12”
        registryCredential = 'dockerhub_id'
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/andrespspano/desafio12.git'
            }
        }
        stage('Linting') {
            steps {
                sh 'hadolint Dockerfile'
            }
        }
        stage('Build Image') {
            steps {
                script {
                    dockerImage = docker.build("andrespspano/desafio13:${env.BUILD_ID}")
                }
            }
        }
        stage('Run Container') {
            steps {
                script {
                    dockerImage.run('-d -p 8081:8081')
                }
            }
        }
        stage('Push to DockerHub') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub-credentials') {
                        dockerImage.push("${env.BUILD_ID}")
                        dockerImage.push("latest")
                    }
                }
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}
