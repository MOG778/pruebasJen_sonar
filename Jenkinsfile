pipeline {
    agent any

    tools {
        // Asegúrate de que 'JDK17' esté configurado en 'Manage Jenkins -> Global Tool Configuration'
        jdk 'JDK17' 
    }

    environment {
        // Asegúrate de que 'SONAR' esté configurado en 'Manage Jenkins -> Configure System'
        SONARQUBE_ENV = 'SONAR' 
        // Asegúrate de que 'SonarScanner' esté configurado con ese nombre en Global Tool Configuration
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

        stage('SonarQube Analysis') {
            steps {
                echo '🚀 Ejecutando análisis con SonarQube...'
                withSonarQubeEnv("${SONARQUBE_ENV}") {
                    // ¡CORRECCIÓN APLICADA AQUÍ!
                    // Se eliminaron las barras invertidas (\) para evitar el "Syntax error: newline unexpected"
                    sh '''
                        ${SCANNER_HOME}/bin/sonar-scanner 
                        -Dsonar.projectKey=pruebasJen_sonar 
                        -Dsonar.projectName=PruebasJenkinsSonar 
                        -Dsonar.projectVersion=1.0 
                        -Dsonar.sources=. 
                        -Dsonar.sourceEncoding=UTF-8 
                        -Dsonar.host.url=http://localhost:9000 
                        -Dsonar.login=<TOKEN_AQUI>
                    '''
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 2, unit: 'MINUTES') {
                    // Espera el resultado del Quality Gate de SonarQube
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
