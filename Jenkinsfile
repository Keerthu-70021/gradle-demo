pipeline {
    agent any

    tools {
        gradle 'Gradle'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build & Test') {
            steps {
                bat 'gradlew.bat clean build jacocoTestReport'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    bat """
                    sonar-scanner ^
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
                archiveArtifacts artifacts: 'build/libs/*.jar'
            }
        }
    }
}
