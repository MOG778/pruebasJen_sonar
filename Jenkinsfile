pipeline {
    // 1. Agente de Ejecución
    agent any

    // 2. Definición de Herramientas
    tools {
        // Asegúrate de que los nombres aquí coincidan exactamente con la configuración global de Jenkins
        jdk 'JDK17' 
    }

    // 3. Variables de Entorno Globales
    environment {
        // Nombre del servidor SonarQube configurado en Jenkins > Administrar > Configurar Sistema
        SONARQUBE_ENV = 'SONAR' 
        
        // Obtenemos la ruta completa del scanner
        SCANNER_HOME = tool name: 'SonarScanner', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
    }

    // 4. Etapas del Pipeline
    stages { 
        
        stage('Checkout') {
            steps {
                echo '📦 Clonando repositorio...'
                // Usa las credenciales del job si están configuradas, o especifica aquí.
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

        stage('SonarQube Analysis') {
            steps {
                echo '🚀 Ejecutando análisis con SonarQube...'
                withSonarQubeEnv("${SONARQUBE_ENV}") {
                    script { // <--- BLOQUE SCRIPT: Necesario para usar 'def' y construir la cadena de forma segura
                        // CORRECCIÓN FINAL: Usamos 127.0.0.1 para forzar IPv4 y evitar el error [0:0:0:0:0:0:0:1]
                        def sonarCommand = "${SCANNER_HOME}/bin/sonar-scanner " + 
                                           "-Dsonar.projectKey=pruebasJen_sonar " + 
                                           "-Dsonar.projectName=PruebasJenkinsSonar " + 
                                           "-Dsonar.projectVersion=1.0 " + 
                                           "-Dsonar.sources=. " + 
                                           "-Dsonar.sourceEncoding=UTF-8 " + 
                                           "-Dsonar.host.url=http://localhost:9000 " + // ¡CAMBIO CLAVE!
                                           "-Dsonar.login=miTokenSecreto" // <--- REEMPLAZA ESTO

                        echo "DEBUG: Comando SH a ejecutar: ${sonarCommand}"
                        // Ejecución del comando
                        sh sonarCommand 
                    }
                }
            }
        }

        stage('Quality Gate') {
            steps {
                echo '⏳ Esperando el Quality Gate de SonarQube...'
                timeout(time: 5, unit: 'MINUTES') { 
                    // Aborta el pipeline si el Quality Gate no se cumple (FAIL/ERROR)
                    waitForQualityGate abortPipeline: true 
                }
            }
        }
    }

    // 5. Post-acciones
    post {
        success {
            echo '🎯 Pipeline finalizado con éxito. El Quality Gate se cumplió.'
        }
        failure {
            echo '❌ Falló el pipeline. El análisis de SonarQube falló o el Quality Gate no se cumplió.'
        }
        always {
            cleanWs()
            echo '🧹 Limpieza del workspace completada.'
        }
    }
}
