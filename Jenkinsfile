pipeline {
    agent any

    tools {
        jdk 'JDK17'          // Nombre configurado en Jenkins
    }

    environment {
        SCANNER_HOME = tool 'SonarScanner'   // Nombre exacto configurado en Jenkins
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
                    ls -l $SCANNER_HOME/bin
                '''
            }
        }

        stage('SonarQube Analysis') {
            steps {
                echo '🚀 Ejecutando análisis con SonarQube...'
                withSonarQubeEnv('SONAR') {
                    sh '''
                        $SCANNER_HOME/bin/sonar-scanner \
                        -Dsonar.projectKey=pruebasJen_sonar \
                        -Dsonar.projectName="Pruebas Jenkins Sonar" \
                        -Dsonar.projectVersion=1.0 \
                        -Dsonar.sources=. \
                        -Dsonar.sourceEncoding=UTF-8
                    '''
                }
                echo '✅ Análisis completado correctamente'
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
