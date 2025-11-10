pipeline {
    agent any

    environment {
        SONAR_TOKEN = credentials('SONAR_TOKEN')  // ton secret Jenkins
        IMAGE_NAME = "myapp:latest"
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/melkihadil1/MyApp.git'
            }
        }

        stage('Build') {
            steps {
                sh 'echo "Build placeholder: mvn clean package ou docker build"'
            }
        }

        stage('SAST - SonarQube Analysis') {
            steps {
                sh 'echo "SonarQube scan placeholder"'
            }
        }

        stage('SCA - Dependency Scan') {
            steps {
                sh 'echo "Trivy scan placeholder"'
            }
        }

        stage('Secrets Scan') {
            steps {
                sh 'echo "Gitleaks scan placeholder"'
            }
        }

        stage('Reporting') {
            steps {
                archiveArtifacts artifacts: '*.txt, *.json', allowEmptyArchive: true
            }
        }
    }
}
