pipeline {
    agent any

    tools {
        jdk 'JDK17'
    }

    environment {
        SONARQUBE_ENV = 'SONAR'
        SCANNER_HOME = tool name: 'SonarScanner', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
    }

    stages { // <--- ESTE BLOQUE ES NECESARIO EN LA RAÍZ
        
        stage('Checkout') {
            steps { // <--- BLOQUE STEPS OBLIGATORIO
                echo '📦 Clonando repositorio...'
                git branch: 'main', url: 'https://github.com/MOG778/pruebasJen_sonar.git'
            }
        }

        stage('Diagnóstico Scanner') {
            steps { // <--- BLOQUE STEPS OBLIGATORIO
                echo '🔍 Verificando instalación de SonarScanner...'
                sh '''
                    echo "Ruta del scanner: $SCANNER_HOME"
                    ls -l "$SCANNER_HOME/bin"
                '''
            }
        }

        stage('SonarQube Analysis') {
            steps { // <--- ¡ASEGÚRATE DE QUE ESTÉ AQUÍ!
                echo '🚀 Ejecutando análisis con SonarQube...'
                withSonarQubeEnv("${SONARQUBE_ENV}") {
                    // Usamos comillas triples dobles ("""...""") y barras invertidas (\) 
                    // para asegurar la expansión de variables y la ejecución de un único comando shell.
                    sh """
                        ${SCANNER_HOME}/bin/sonar-scanner \\
                        -Dsonar.projectKey=pruebasJen_sonar \\
                        -Dsonar.projectName=PruebasJenkinsSonar \\
                        -Dsonar.projectVersion=1.0 \\
                        -Dsonar.sources=. \\
                        -Dsonar.sourceEncoding=UTF-8 \\
                        -Dsonar.host.url=http://localhost:9000 \\
                        -Dsonar.login=<TOKEN_AQUI>
                    """
                }
            }
        }

        stage('Quality Gate') {
            steps { // <--- BLOQUE STEPS OBLIGATORIO
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
