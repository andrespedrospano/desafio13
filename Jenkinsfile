pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-credentials')
        DOCKER_IMAGE = 'andrespedrospano/desafio13:v1'
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/andrespedrospano/desafio13.git'
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
                    docker.build("${env.DOCKER_IMAGE}")
                }
            }
        }
        stage('Run Container') {
            steps {
                script {
                     docker.image("${env.DOCKER_IMAGE}").inside {
                        sh 'echo "Container is running"'
                    }    
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
