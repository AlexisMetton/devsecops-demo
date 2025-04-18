pipeline {
  agent any

  environment {
    SONAR_TOKEN = credentials('sonar-token')
  }

  stages {
    stage('Clone') {
      steps {
        git 'https://github.com/AlexisMetton/devsecops-demo.git'
      }
    }

    stage('Build Docker Image') {
      steps {
        sh 'docker build -t devsecops-demo:test .'
      }
    }

    stage('Analyse SonarQube') {
      steps {
        withSonarQubeEnv('My SonarQube Server') {
          script {
            def scannerHome = tool 'sonar-scanner'
            sh "${scannerHome}/bin/sonar-scanner -Dsonar.login=$SONAR_TOKEN"
          }
        }
      }
    }

    stage('Scan Trivy') {
      steps {
        sh 'trivy image devsecops-demo:test || true'
      }
    }
  }
}
