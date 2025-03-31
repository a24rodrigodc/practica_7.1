pipeline {
    agent {
        docker {
            image 'node:18-alpine'  // Imaxe lixeira con Node.js 18
            args '-u root'          // Executar como root para evitar problemas de permisos
        }
    }
    stages {
        stage('Instalar dependencias') {
            steps {
                sh 'npm install'
            }
        }
        
        stage('Executar tests') {
            steps {
                sh 'npm run test'
            }
        }
    }
    
    post {
        always {
            echo '🔹 Pipeline completado'
        }
        success {
            echo '✅ Todos os pasos completáronse correctamente'
        }
        failure {
            echo '❌ O pipeline fallou. Consulta os logs para máis detalles'
        }
    }
}
