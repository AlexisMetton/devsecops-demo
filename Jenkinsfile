pipeline {
  agent any

  environment {
    SONARQUBE_URL = 'http://sonarqube:9000'
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
          sh 'sonar-scanner -Dsonar.login=$SONAR_TOKEN'
        }
      }
    }

    stage('Scan Trivy') {
      steps {
        sh 'docker run --rm -v /var/run/docker.sock:/var/run/docker.sock aquasec/trivy image devsecops-demo:test'
      }
    }
  }
}
