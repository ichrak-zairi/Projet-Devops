pipeline {
    agent any

    environment {
        MVN_HOME = "C:/apache-maven-3.8.8" // ou le chemin où Maven est installé
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/ichrak-zairi/Projet-Devops.git'
            }
        }

        stage('Build') {
            steps {
                bat "${env.MVN_HOME}/bin/mvn clean compile"
            }
        }

        stage('Test') {
            steps {
                bat "${env.MVN_HOME}/bin/mvn test"
            }
        }

        stage('Package') {
            steps {
                bat "${env.MVN_HOME}/bin/mvn package"
            }
        }

        stage('SonarQube Analysis') {
            environment {
                SONARQUBE = "SonarQube" // nom du serveur SonarQube configuré dans Jenkins
            }
            steps {
                withSonarQubeEnv('SonarQube') {
                    bat "${env.MVN_HOME}/bin/mvn sonar:sonar"
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
