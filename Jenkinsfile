pipeline {
    agent any

    tools {
        // Asegúrate de tener estas herramientas configuradas en Jenkins (Manage Jenkins -> Global Tool Configuration)
        maven 'Maven3'
        jdk 'JDK17'
        hudson.plugins.sonar.SonarRunnerInstallation 'SonarScanner'
    }

    environment {
        // Nombre del servidor configurado en Jenkins -> Manage Jenkins -> Configure System -> SonarQube Servers
        SONARQUBE_ENV = 'SonarQube'
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/MOG778/pruebasJen_sonar.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv("${SONARQUBE_ENV}") {
                    sh 'mvn sonar:sonar'
                }
            }
        }

        stage('Quality Gate') {
            steps {
                script {
                    timeout(time: 3, unit: 'MINUTES') {
                        waitForQualityGate abortPipeline: true
                    }
                }
            }
        }
    }
}
