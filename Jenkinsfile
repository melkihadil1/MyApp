pipeline {
    agent any

    environment {
        // Met ton token ici
        SONAR_TOKEN = credentials('SONAR_LOCAL_TOKEN')
        SONAR_HOST_URL = "http://localhost:9000"
        MAVEN_OPTS = "-Dmaven.test.skip=true"
    }

    stages {
        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo "Building the project..."
                sh "mvn clean package -DskipTests"
            }
        }

        stage('Test') {
            steps {
                echo "Running tests..."
                sh "mvn test"
            }
        }

        stage('SAST - SonarQube Analysis') {
            environment {
                // Injection des variables SonarQube
                SONAR_PROJECT_KEY = "melkihadil1_MyApp"
            }
            steps {
                echo "Running SonarQube analysis..."
                sh """
                    mvn sonar:sonar \
                    -Dsonar.projectKey=$SONAR_PROJECT_KEY \
                    -Dsonar.host.url=$SONAR_HOST_URL \
                    -Dsonar.login=$SONAR_TOKEN
                """
            }
        }

        // Tu peux ajouter d'autres étapes comme SCA, Docker Scan, Secrets Scan, etc.
    }

    post {
        always {
            echo 'Pipeline terminé — toutes les étapes exécutées.'
        }
        success {
            echo 'Build et analyse SonarQube réussis.'
        }
        failure {
            echo 'Échec du pipeline. Vérifie les logs pour plus de détails.'
        }
    }
}

