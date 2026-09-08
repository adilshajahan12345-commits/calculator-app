pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build & Test') {
            steps {
                sh 'mvn clean test'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh 'mvn sonar:sonar'
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 5, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }

    post {
        success {
            emailext(
                subject: "SUCCESS: Calculator App - ${env.BUILD_NUMBER}",
                body: "SonarQube Quality Gate PASSED.\nBuild: ${env.BUILD_NUMBER}",
                to: "adilshajahan777gmail.com"
            )
        }

        failure {
            emailext(
                subject: "FAILED: Calculator App - ${env.BUILD_NUMBER}",
                body: "Build or SonarQube Quality Gate FAILED.\nBuild: ${env.BUILD_NUMBER}",
                to: "adilshajahan777gmail.com"
            )
        }
    }
}
