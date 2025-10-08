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
                sh "${MVN_HOME}/bin/mvn clean"
            }
        }

        stage('Compile') {
            steps {
                echo '⚙️ Compilation du projet...'
                sh "${MVN_HOME}/bin/mvn compile"
            }
        }

        stage('Test') {
            steps {
                echo '🧪 Exécution des tests...'
                sh "${MVN_HOME}/bin/mvn test"
            }
        }

        stage('SonarQube Analysis') {
            steps {
                echo '🔍 Analyse de la qualité du code avec SonarQube...'
                withSonarQubeEnv('sonar-token') {
                    sh "${MVN_HOME}/bin/mvn sonar:sonar -Dsonar.projectKey=Projet-Devops -Dsonar.host.url=http://192.168.33.10:9000 -Dsonar.login=${SONAR_TOKEN}"
                }
            }
        }

        stage('Package') {
            steps {
                echo '📦 Génération du fichier JAR...'
                sh "${MVN_HOME}/bin/mvn package -DskipTests"
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
