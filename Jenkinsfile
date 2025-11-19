pipeline {
    agent any

    tools {
        jdk 'JDK17'
    }

    environment {
        SONARQUBE_ENV = 'SONAR'
        SCANNER_HOME = tool name: 'SonarQ', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
    }

    stages {

        stage('Checkout') {
            steps {
                echo '📦 Clonando repositorio...'
                git branch: 'main', credentialsId: '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', url: 'https://github.com/MOG778/pruebasJen_sonar.git/'
            }
        }

        stage('Diagnóstico Scanner') {
            steps {
                echo '🔍 Verificando instalación de SonarScanner...'
                sh """
                    echo Ruta del scanner: $SCANNER_HOME
                    ls -l $SCANNER_HOME/bin
                """
            }
        }

        stage('SonarQube Analysis') {
            steps {
                echo '🚀 Ejecutando análisis con SonarQube...'
                withSonarQubeEnv("${SONARQUBE_ENV}") {
                    sh """
                        $SCANNER_HOME/bin/sonar-scanner \
                        -Dsonar.projectKey=pruebasJen_sonar \
                        -Dsonar.projectName=PruebasJenkinsSonar \
                        -Dsonar.projectVersion=1.0 \
                        -Dsonar.sources=. \
                        -Dsonar.sourceEncoding=UTF-8 \
                        -Dsonar.host.url=http://172.18.0.1:9000 \
                        -Dsonar.login=sqa_f19355173fb0c988b1416b1a9aa96ec95543e45d
                    """
                }
            }
        }

        stage('Quality Gate') {
            steps {
                echo '⏳ Esperando Quality Gate...'
                timeout(time: 5, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }

    post {
        success {
            echo '🎯 Éxito total — Quality Gate OK'
        }
        failure {
            echo '❌ Falló el pipeline'
        }
        always {
            cleanWs()
            echo '🧹 Workspace limpio.'
        }
    }
}
