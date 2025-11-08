pipeline {
    agent any

    environment {
        SONARQUBE_ENV = 'SONAR'          // nombre del servidor configurado en Jenkins > SonarQube servers
        SCANNER_HOME = tool 'SonarScanner' // nombre exacto de la instalación del scanner en Jenkins
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Clonando repositorio...'
                git branch: 'main', url: 'https://github.com/MOG778/pruebasJen_sonar.git'
            }
        }

        stage('Diagnóstico Scanner') {
            steps {
                echo '=== Diagnóstico SonarScanner ==='
                sh 'echo Ruta del scanner: $SCANNER_HOME'
                sh 'ls -l $SCANNER_HOME/bin'
                echo '================================='
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv("${SONARQUBE_ENV}") {
                    script {
                        // Construimos el comando limpio, sin saltos de línea
                        def scannerCmd = "${SCANNER_HOME}/bin/sonar-scanner " +
                                         "-Dsonar.projectKey=pruebasJen_sonar " +
                                         "-Dsonar.sources=. " +
                                         "-Dsonar.host.url=http://sonarqube:9000 " +
                                         "-Dsonar.login=<TOKEN_AQUI>"

                        // Mostramos el comando que se ejecutará
                        sh "echo Ejecutando: ${scannerCmd}"

                        // Ejecutamos el análisis
                        sh label: 'Run SonarScanner', script: scannerCmd
                    }
                }
            }
        }

        stage('Quality Gate') {
            steps {
                script {
                    echo 'Esperando resultado del Quality Gate...'
                    timeout(time: 3, unit: 'MINUTES') {
                        def qg = waitForQualityGate abortPipeline: true
                        echo "Resultado del Quality Gate: ${qg.status}"
                    }
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline finalizado. Limpieza del workspace.'
            cleanWs()
        }
        failure {
            echo '❌ Falló el pipeline. Revisa los logs arriba.'
        }
        success {
            echo '✅ Análisis completado con éxito y enviado a SonarQube.'
        }
    }
}
