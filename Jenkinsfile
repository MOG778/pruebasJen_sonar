pipeline {
    agent any

    environment {
        SONARQUBE = credentials('sonar-token')
    }

    tools {
        sonarScanner 'SonarScanner'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/MOG778/pruebasJen_sonar.git'
            }
        }

        stage('Analizar con SonarQube') {
            steps {
                echo "🚀 Ejecutando análisis SonarQube..."
                withSonarQubeEnv('SonarQube') {
                    sh '''
                        sonar-scanner \
                        -Dsonar.projectKey=pruebasJen_sonar \
                        -Dsonar.sources=. \
                        -Dsonar.host.url=$SONAR_HOST_URL \
                        -Dsonar.login=$SONARQUBE
                    '''
                }
            }
        }
    }

    post {
        success {
            echo "✅ Análisis enviado correctamente a SonarQube"
        }
        failure {
            echo "❌ Falló el análisis SonarQube, revisa los logs"
        }
    }
}
