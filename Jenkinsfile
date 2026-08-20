pipeline {
    agent any
    environment {
        IMAGE_NAME_BACKEND = 'khadidiatou12/smarttask-backend'
        IMAGE_NAME_FRONTEND = 'khadidiatou12/smarttask-frontend'
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Docker Build and Push') {
            steps {
                script {
                    def tag = env.BRANCH_NAME == 'Prod' ? 'latest' : 'dev'
                    
                    // Connexion, construction et envoi sécurisés via les identifiants Jenkins
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', 
                                                      usernameVariable: 'DOCKER_USER', 
                                                      passwordVariable: 'DOCKER_PASS')]) {
                        
                        // 1. Connexion à Docker Hub
                        sh "echo ${DOCKER_PASS} | docker login -u ${DOCKER_USER} --password-stdin"
                        
                        // 2. Build et Push Backend
                        sh "docker build -t ${IMAGE_NAME_BACKEND}:${tag} ./backend"
                        sh "docker push ${IMAGE_NAME_BACKEND}:${tag}"
                        
                        // 3. Build et Push Frontend
                        sh "docker build -t ${IMAGE_NAME_FRONTEND}:${tag} ./frontend"
                        sh "docker push ${IMAGE_NAME_FRONTEND}:${tag}"
                    }
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