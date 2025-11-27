pipeline {
    agent any

    stages {
        stage('Récupérer le code') {
            steps {
                echo 'Récupération du code GitHub'
                checkout scm
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
                sh 'docker tag souhakhelifi/student-management:latest souhakhelifi/student-management:2'
            }
        }

        stage('Push sur Docker Hub') {
            steps {
                // Utilise exactement l'ID de ta credential : dockerhub-souha
                withCredentials([usernamePassword(credentialsId: 'dockerhub-souha', 
                                                 usernameVariable: 'DOCKER_USER', 
                                                 passwordVariable: 'DOCKER_PASS')]) {
                    sh 'echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin'
                    sh 'docker push souhakhelifi/student-management:latest'
                    sh 'docker push souhakhelifi/student-management:2'
                }
            }
        }
    }

    post {
        always {
            // Nettoyage des images locales après le build
            sh 'docker rmi souhakhelifi/student-management:latest || true'
            sh 'docker rmi souhakhelifi/student-management:2 || true'
            sh 'docker logout || true'
        }
        success {
            echo 'Tout est bon ! Image poussée sur Docker Hub avec les tags latest et 2'
        }
    }
}
