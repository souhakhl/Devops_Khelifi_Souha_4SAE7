pipeline {
    agent any

    environment {
        DOCKERHUB_USER        = 'souhakhelifi'
        IMAGE_NAME            = "${DOCKERHUB_USER}/student-management"
        DOCKERHUB_CREDENTIALS = 'dockerhub-souha'
    }

    stages {
        stage('Création image Docker') {
            steps {
                sh 'docker build -t ${IMAGE_NAME}:latest .'
                sh 'docker tag ${IMAGE_NAME}:latest ${IMAGE_NAME}:${BUILD_NUMBER}'
            }
        }

        stage('Push de l\'image sur DockerHub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: '${DOCKERHUB_CREDENTIALS}',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                        echo "$DOCKER_PASS" | docker login --username "$DOCKER_USER" --password-stdin
                        docker push ${IMAGE_NAME}:latest
                        docker push ${IMAGE_NAME}:${BUILD_NUMBER}
                    '''
                }
            }
        }
    }

    post {
        always {
            sh '''
                docker rmi ${IMAGE_NAME}:latest || true
                docker rmi ${IMAGE_NAME}:${BUILD_NUMBER} || true
            '''
        }
    }
}
