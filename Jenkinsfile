pipeline {
    agent any

    tools {
        jdk 'JDK17'
    }

    environment {
        SONARQUBE_ENV = 'SONAR'
        SCANNER_HOME = tool name: 'SonarScanner', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
    }

    stages { 
        
        stage('Checkout') {
            steps {
                echo '📦 Clonando repositorio...'
                git branch: 'main', url: 'https://github.com/MOG778/pruebasJen_sonar.git'
            }
        }

        stage('Diagnóstico Scanner') {
            steps {
                echo '🔍 Verificando instalación de SonarScanner...'
                sh '''
                    echo "Ruta del scanner: $SCANNER_HOME"
                    ls -l "$SCANNER_HOME/bin"
                '''
            }
        }

// Definir una variable para el comando completo para depuración
def sonarCommand = "${SCANNER_HOME}/bin/sonar-scanner -Dsonar.projectKey=pruebasJen_sonar -Dsonar.projectName=PruebasJenkinsSonar -Dsonar.projectVersion=1.0 -Dsonar.sources=. -Dsonar.sourceEncoding=UTF-8 -Dsonar.host.url=http://localhost:9000 -Dsonar.login=<TOKEN_AQUI>"

stage('SonarQube Analysis') {
    steps {
        echo '🚀 Ejecutando análisis con SonarQube...'
        withSonarQubeEnv("${SONARQUBE_ENV}") {
            // SOLUCIÓN FINAL: Ejecución explícita del comando completo.
            // Primero, imprimimos el comando para verificar que sea correcto.
            echo "DEBUG: Comando SH a ejecutar: ${sonarCommand}"
            
            // Usamos 'sh' con el comando ya construido en una variable.
            sh sonarCommand 
        }
    }
}
        stage('Quality Gate') {
            steps {
                timeout(time: 2, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }

    post {
        success {
            echo '🎯 Pipeline finalizado con éxito. Código analizado correctamente.'
            cleanWs()
        }
        failure {
            echo '❌ Falló el pipeline. Revisa los logs arriba.'
            cleanWs()
        }
    }
}
