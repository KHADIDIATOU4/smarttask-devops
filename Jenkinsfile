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
        stage('Docker Build') {
            steps {
                script {
                    def tag = env.BRANCH_NAME == 'Prod' ? 'latest' : 'dev'
                    sh "docker build -t ${IMAGE_NAME_BACKEND}:${tag} ./backend"
                    sh "docker build -t ${IMAGE_NAME_FRONTEND}:${tag} ./frontend"
                }
            }
        }
        stage('Docker Push') {
            steps {
                script {
                    def tag = env.BRANCH_NAME == 'Prod' ? 'latest' : 'dev'
                    
                    // Utilisation sécurisée des identifiants Jenkins
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', 
                                                      usernameVariable: 'DOCKER_USER', 
                                                      passwordVariable: 'DOCKER_PASS')]) {
                        sh "echo ${DOCKER_PASS} | docker login -u ${DOCKER_USER} --password-stdin"
                        sh "docker push ${IMAGE_NAME_BACKEND}:${tag}"
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