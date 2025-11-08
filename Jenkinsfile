stage('SonarQube Analysis') {
    steps {
        echo '🚀 Ejecutando análisis con SonarQube...'
        withSonarQubeEnv("${SONARQUBE_ENV}") {
            // USAMOS COMILLAS TRIPLES DOBLES ("""...""") para permitir la expansión de variables Groovy
            // USAMOS BARRAS INVERTIDAS (\) para asegurar que todo se ejecute como UN SOLO COMANDO 'sh'
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
