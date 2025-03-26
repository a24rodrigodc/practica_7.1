pipeline {
  environment {
    IMAGEN = "cparisfp/myapp"
    BUILD_NUMBER = "1"
    USUARIO = 'USER_DOCKERHUB'
  }
  agent any
  stages {
    stage('Clone') {
      steps {
        git branch: "main", url: 'https://github.com/cristina-paris/practicagithubactions'
      }
    }
    stage('Build') {
      steps {
        script {
          newApp = docker.build "$IMAGEN:$BUILD_NUMBER"
        }
      }
    }
    stage('Deploy') {
      steps {
        script {
          docker.withRegistry( '', USUARIO ) {
            newApp.push()
          }
        }
      }    
    }
    stage('Clean Up') {
      steps {
        sh "docker rmi $IMAGEN:$BUILD_NUMBER"
      }
    }
  }
}
