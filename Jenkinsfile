pipeline {
    agent any

    environment {
        // Path to SonarScanner (adjust to your installation)
        SONAR_SCANNER_HOME = "C:\\sonarscanner\\sonar-scanner-8.0.1.6346-windows-x64\\bin"
        PATH = "${env.SONAR_SCANNER_HOME};${env.PATH}"
    }

    stages {
        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }

        stage('Build & Test') {
            steps {
                // Use Gradle wrapper from your project
                bat 'gradlew.bat clean build jacocoTestReport'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') { // Name of your SonarQube server in Jenkins
                    bat """
                        ${env.SONAR_SCANNER_HOME}\\sonar-scanner.bat ^
                        -Dsonar.projectKey=gradle-demo ^
                        -Dsonar.projectName=gradle-demo ^
                        -Dsonar.sources=src/main/java ^
                        -Dsonar.tests=src/test/java ^
                        -Dsonar.java.binaries=build/classes ^
                        -Dsonar.coverage.jacoco.xmlReportPaths=build/reports/jacoco/test/jacocoTestReport.xml
                    """
                }
            }
        }

        stage('Archive Artifact') {
            steps {
                archiveArtifacts artifacts: 'build/libs/*.jar', fingerprint: true
            }
        }
    }

    post {
        success {
            echo "Pipeline completed successfully. JAR archived and SonarQube analysis done!"
        }
        failure {
            echo "Pipeline failed. Check logs for details."
        }
    }
}
