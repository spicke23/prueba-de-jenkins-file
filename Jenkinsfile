pipeline {
    agent any 

    tools { 
        maven 'jenkinsmaven'
        jdk 'javajenkins'
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Clonando repositorio desde GitHub'
                git branch: 'main', url: 'https://github.com/spicke23/prueba-de-jenkins-file.git'
            }
        }

        stage('Build') {
            steps {
                echo 'Ejecutando Maven (limpieza de directorio y empaquetamiento de la aplicacion)'
                script {
                    sh 'mvn clean package'
                }
            }
        }

        stage('Archive Artifacts') {
            steps {
                echo 'Almacenamiento del artefacto en el directorio TARGET'
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }

        stage('Test') {
            steps {
                echo 'Ejecutando test de Maven'
                script {
                    sh 'mvn test'
                }
            }
        }

        stage('Sonar Scanner') {
            steps {
                echo 'Ejecutando analisis desde SonarQube'
                script {
                    def sonarqubeScannerHome = tool name: 'sonar', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
                    withCredentials([string(credentialsId: 'sonar', variable: 'sonarLogin')]) {
                        sh "${sonarqubeScannerHome}/bin/sonar-scanner -e -Dsonar.host.url=http://SonarQube:9000 -Dsonar.login=${sonarLogin} -Dsonar.projectName=mv-maven -Dsonar.projectVersion=${env.BUILD_NUMBER} -Dsonar.projectKey=GS -Dsonar.sources=src/main/java/com/kibernumacademy/devops -Dsonar.tests=src/test/java/com/kibernumacademy/devops -Dsonar.language=java -Dsonar.java.binaries=."
                    }
                }
            }
        }
    }
}
