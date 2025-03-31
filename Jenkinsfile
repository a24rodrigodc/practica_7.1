pipeline {
    agent {
         docker {
             image 'node:18-alpine'  // Imaxe lixeira con Node.js 18
             args '-u root'         // Executar como root para evitar problemas de permisos
         }
    }
    environment {
        IMAGE_NAME = "a24rodrigodc/nuse"
        DOCKERHUB_CREDENTIALS = credentials('USER_DOCKERHUB')
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install dependencies and run tests') {
            steps {
                sh 'npm install'
                sh 'npm test'
            }
        }

        stage('Build Docker image') {
            agent any
            when {
                expression { currentBuild.result == null || currentBuild.result == 'SUCCESS' }
            }
            steps {
                script {
                    docker.build("${IMAGE_NAME}")
                }
            }
        }

        stage('Push to Docker Hub') {
            agent any
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub-credentials') {
                        docker.image("${IMAGE_NAME}").push('latest')
                    }
                }
            }
        }
    }

    post {
        failure {
            echo 'Pipeline failed!'
        }
        success {
            echo 'Pipeline succeeded! Image pushed to Docker Hub.'
        }
    }
}
