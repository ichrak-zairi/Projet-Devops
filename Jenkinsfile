pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/ichrak-zairi/Projet-Devops.git'
            }
        }

        stage('Build') {
            steps {
                sh "mvn clean compile"
            }
        }

        stage('Test') {
            steps {
                sh "mvn test"
            }
        }

        stage('Package') {
            steps {
                sh "mvn package"
            }
        }

        stage('SonarQube Analysis') {
            environment {
                SONARQUBE = "SonarQube" // nom du serveur SonarQube configuré dans Jenkins
            }
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh "mvn sonar:sonar"
                }
            }
        }
    }

    post {
        success {
            echo 'Build et analyse SonarQube réussis ✅'
        }
        failure {
            echo 'Le build a échoué ❌'
        }
    }
}
