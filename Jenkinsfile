stage('SonarQube Analysis') {
    steps {
        echo '🚀 Ejecutando análisis con SonarQube...'
        withSonarQubeEnv("${SONARQUBE_ENV}") {
            script {
                // CORRECCIÓN: Usar 127.0.0.1 en lugar de localhost para forzar IPv4
                def sonarCommand = "${SCANNER_HOME}/bin/sonar-scanner " + 
                                   "-Dsonar.projectKey=pruebasJen_sonar " + 
                                   "-Dsonar.projectName=PruebasJenkinsSonar " + 
                                   "-Dsonar.projectVersion=1.0 " + 
                                   "-Dsonar.sources=. " + 
                                   "-Dsonar.sourceEncoding=UTF-8 " + 
                                   "-Dsonar.host.url=http://127.0.0.1:9000 " + // <--- ¡CAMBIO CLAVE AQUÍ!
                                   "-Dsonar.login=miTokenSecreto"

                echo "DEBUG: Comando SH a ejecutar: ${sonarCommand}"
                sh sonarCommand 
            }
        }
    }
}
