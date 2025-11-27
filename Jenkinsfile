pipeline {
    agent any

    stages {
        stage('Récupérer le code') {
            steps {
                echo 'Je récupère le code GitHub'
            }
        }

        stage('Construire le JAR') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Créer l’image Docker') {
            steps {
                sh 'docker build -t souhakhelifi/student-management:latest .'
            }
        }

        stage('Envoyer sur Docker Hub') {
            steps {
                sh 'echo "$DOCKER_PASS" | docker login -u souhakhelifi --password-stdin'
                sh 'docker push souhakhelifi/student-management:latest'
            }
        }
    }
}