pipeline {
    agent any

    environment {
        SONARQUBE_ENV = 'SONAR'            // nombre del servidor configurado en Jenkins
        SCANNER_HOME = tool 'SonarScanner' // nombre exacto del scanner en Jenkins
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
                sh 'echo "Ruta del scanner: $SCANNER_HOME"'
                sh 'ls -l $SCANNER_HOME/bin'
                echo '================================='
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv("${SONARQUBE_ENV}") {
                    sh """
                        echo "Ejecutando análisis con SonarQube..."
                        ${SCANNER_HOME}/bin/sonar-scanner \
                            -Dsonar.projectKey=pruebasJen_sonar \
                            -Dsonar.sources=. \
                            -Dsonar.host.url=http://sonarqube:9000 \
                            -Dsonar.login=<TOKEN_AQUI>
                    """
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
            echo '✅ Análisis completado co
