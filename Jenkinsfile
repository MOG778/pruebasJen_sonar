pipeline {
    agent any

    stages {
        stage('Inicio') {
            steps {
                echo '🔥 Jenkins está ejecutando correctamente el pipeline'
            }
        }

        stage('Prueba de Shell') {
            steps {
                sh 'echo "🧠 Esto se está ejecutando dentro del contenedor Jenkins"'
                sh 'uname -a'
            }
        }

        stage('Listar archivos del workspace') {
            steps {
                sh 'ls -la'
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline ejecutado con éxito'
        }
        failure {
            echo '❌ Algo falló, revisa los logs'
        }
    }
}
