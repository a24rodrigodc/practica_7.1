pipeline {
    agent none

    environment {
        IMAGE_NAME = "a24rodrigodc/nuse"
        DOCKERHUB_CREDENTIALS = credentials('USER_DOCKERHUB')  
    }

    stages {
        stage('Checkout') {
            agent {
                docker {
                    image 'node:18-alpine'
                    args '-u root'
                }
            }
            steps {
                checkout scm
            }
        }

        stage('Install dependencies and run tests') {
            agent {
                docker {
                    image 'node:18-alpine'
                    args '-u root'
                }
            }
            steps {
                sh 'npm install'
                sh 'npm test'
            }
        }
        stage('Build and Push Docker Image') {
            agent any
            when {
                expression { currentBuild.result == null || currentBuild.result == 'SUCCESS' }
            }
            steps {
                script {
                    docker.build("${IMAGE_NAME}")

                    docker.withRegistry('https://registry.hub.docker.com', 'USER_DOCKERHUB') {
                        docker.image("${IMAGE_NAME}").push('latest')
                    }
                }
            }
        }
    }

    post {
        failure {
            echo 'Pipeline failed! Revisa los logs.'
        }
        success {
            echo '¡Éxito! Imagen subida a Docker Hub.'
        }
    }
}
