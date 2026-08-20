pipeline {
    agent any
    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }
        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                }
            }
        }
        stage('Build and Push Backend') {
            steps {
                script {
                    // Remplace "backend" par le nom de ton dossier backend si besoin
                    app = docker.build("khadidiatou12/smarttask-backend:latest", "./backend")
                    docker.withRegistry('', 'dockerhub-credentials') {
                        app.push()
                    }
                }
            }
        }
        stage('Build and Push Frontend') {
            steps {
                script {
                    // Remplace "frontend" par le nom de ton dossier frontend si besoin
                    app = docker.build("khadidiatou12/smarttask-frontend:latest", "./frontend")
                    docker.withRegistry('', 'dockerhub-credentials') {
                        app.push()
                    }
                }
            }
        }
    }
}