pipeline {
    agent any

    environment {
        SONARQUBE = 'SonarQubeServer'      // Nom exact du serveur SonarQube configuré dans Jenkins
        SONAR_TOKEN = credentials('SONAR_LOCAL_TOKEN')  // Jenkins Credentials Secret Text pour le token local
        IMAGE_NAME = "myapp:latest"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/melkihadil1/MyApp.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('SAST - SonarCloud / SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQubeServer') {
                    sh """
                    mvn sonar:sonar \
                        -Dsonar.projectKey=melkihadil1_MyApp \
                        -Dsonar.host.url=http://192.168.17.174:9000 \
                        -Dsonar.login=$SONAR_TOKEN
                    """
                }
            }
        }

        stage('SCA - Dependency Scan') {
            steps {
                sh 'trivy fs . > trivy-report.txt || true'
            }
        }

        stage('Docker Build & Scan') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
                sh 'trivy image $IMAGE_NAME > docker-scan.txt || true'
            }
        }

        stage('Secrets Scan') {
            steps {
                sh 'gitleaks detect --source . --report-format=json --report-path=gitleaks.json || true'
            }
        }

        stage('DAST - Staging Scan (Optionnel)') {
            steps {
                sh 'echo "DAST scan placeholder (ex: OWASP Zap, Nikto, etc.)"'
            }
        }

        stage('Archive & Reporting') {
            steps {
                archiveArtifacts artifacts: '*.txt, *.json', allowEmptyArchive: true
                emailext to: 'team@devsecops.com',
                         subject: "Résultats du pipeline Jenkins DevSecOps",
                         body: "Les rapports sont disponibles dans Jenkins."
            }
        }
    }

    post {
        always {
            echo "Pipeline terminé — toutes les étapes exécutées."
        }
    }
}
