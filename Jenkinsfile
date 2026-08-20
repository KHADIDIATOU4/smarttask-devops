pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-credentials')
        IMAGE_NAME_BACKEND = 'khadidiatou12/smarttask-backend'
        IMAGE_NAME_FRONTEND = 'khadidiatou12/smarttask-frontend'
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Fix Docker Socket Permissions') {
            steps {
                // Donne automatiquement les droits sur le socket au démarrage de l'étape
                sh 'chmod 666 /var/run/docker.sock || true'
            }
        }
        stage('Docker Build and Push') {
            steps {
                script {
                    def tag = env.BRANCH_NAME == 'Prod' ? 'latest' : 'dev'
                    
                    // Connexion Docker Hub
                    sh "echo ${DOCKERHUB_CREDENTIALS_PSW} | docker login -u ${DOCKERHUB_CREDENTIALS_USR} --password-stdin"
                    
                    // Build et Push Backend
                    sh "docker build -t ${IMAGE_NAME_BACKEND}:${tag} ./backend"
                    sh "docker push ${IMAGE_NAME_BACKEND}:${tag}"
                    
                    // Build et Push Frontend
                    sh "docker build -t ${IMAGE_NAME_FRONTEND}:${tag} ./frontend"
                    sh "docker push ${IMAGE_NAME_FRONTEND}:${tag}"
                }
            }
        }
    }
    post {
        success {
            echo 'Pipeline exécuté avec succès ! Images poussées sur Docker Hub.'
        }
        failure {
            echo 'Le pipeline a échoué. Vérifiez les logs.'
        }
    }
}