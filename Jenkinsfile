pipeline {
    agent any

    environment {
        MVN_HOME = tool 'Maven'
        JAVA_HOME = tool 'JDK17'
        PATH = "${JAVA_HOME}/bin:${MVN_HOME}/bin:${env.PATH}"
        SONAR_TOKEN = credentials('sonar-token')
    }

    stages {

        stage('Pull from Git') {
            steps {
                echo '📥 Récupération du code source depuis GitHub...'
                git branch: 'main', url: 'https://github.com/ichrak-zairi/Projet-Devops.git', credentialsId: 'git-credentials'
            }
        }

        stage('Clean') {
            steps {
                echo '🧹 Nettoyage du projet...'
                sh 'mvn clean'
            }
        }

        stage('Compile') {
            steps {
                echo '⚙️ Compilation du projet...'
                sh 'mvn compile'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                echo '🔍 Analyse de la qualité du code avec SonarQube...'
                withSonarQubeEnv('sonar-server') {
                    sh "mvn sonar:sonar -Dsonar.projectKey=Projet-Devops -Dsonar.host.url=http://sonarqube:9000 -Dsonar.login=${SONAR_TOKEN}"
                }
            }
        }

        stage('Package') {
            steps {
                echo '📦 Génération du fichier JAR...'
                sh 'mvn package -DskipTests'
            }
            post {
                success {
                    archiveArtifacts artifacts: 'target/*.jar', allowEmptyArchive: false
                }
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline terminé avec succès !'
        }
        failure {
            echo '❌ Échec du pipeline. Vérifie les logs pour plus de détails.'
        }
    }
}
