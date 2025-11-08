pipeline {
    // 1. Agente de Ejecución
    agent any

    // 2. Definición de Herramientas
    tools {
        jdk 'JDK17' // Asegúrate de que este nombre coincida con la configuración de Jenkins
        // Asegúrate de que 'SonarScanner' coincida con el nombre de la instalación configurada
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
                // Usamos la configuración de Git SCM del pipeline, o si es Declarativo:
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
                    script { // <--- Bloque 'script' para manejar lógica Groovy y variables 'def'
                        // Construimos el comando completo en una variable Groovy para garantizar una cadena limpia
                        def sonarCommand = "${SCANNER_HOME}/bin/sonar-scanner " + 
                                           "-Dsonar.projectKey=pruebasJen_sonar " + 
                                           "-Dsonar.projectName=PruebasJenkinsSonar " + 
                                           "-Dsonar.projectVersion=1.0 " + 
                                           "-Dsonar.sources=. " + 
                                           "-Dsonar.sourceEncoding=UTF-8 " + 
                                           "-Dsonar.host.url=http://localhost:9000 " + 
                                           "-Dsonar.login=miTokenSecreto" // <--- REEMPLAZA ESTO CON TU TOKEN REAL

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
                timeout(time: 5, unit: 'MINUTES') { // Damos 5 minutos para el análisis
                    // Aborta el pipeline si el Quality Gate no se cumple
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
