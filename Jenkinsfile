pipeline {
    agent {
        docker {
            image 'circleci/openjdk:10-jdk-node'
        }
    }
    stages {
        stage('Prepare') {
            steps {
                checkout scm
            }
        }
        stage('Test') {
            steps {
                sh 'mvn install'
            }
        }
        stage('QA') {
          stage('Sonarqube') {
            environment {
              scannerHome = tool 'SonarQubeScanner'
            }
            steps {
              withSonarQubeEnv('sonarqube') {
                sh "${scannerHome}/bin/sonar-scanner"
              }
              timeout(time: 30, unit: 'MINUTES') {
                waitForQualityGate abortPipeline: true
              }
            }
          }
        }
    }
}