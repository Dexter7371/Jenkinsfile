pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = 'dockerhub-creds-id'
        GITHUB_CREDENTIALS = '53736394-88a2-4c62-a661-7187785b45aa'
        IMAGE_NAME = 'dexter7371/frontend-angular-19'
    }

    stages {
        stage('Checkout Jenkinsfile Repo') {
            steps {
                // This was already successful
                git credentialsId: "${GITHUB_CREDENTIALS}", branch: 'main', url: 'https://github.com/dexter73710/Jenkinsfile.git'
            }
        }

        stage('Checkout Angular Project') {
            steps {
                git credentialsId: "${GITHUB_CREDENTIALS}", branch: 'main', url: 'https://github.com/dexter73710/frontend-angular-19.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${IMAGE_NAME}", '.')
                }
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: "${DOCKERHUB_CREDENTIALS}", usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh 'echo "$PASSWORD" | docker login -u "$USERNAME" --password-stdin'
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.image("${IMAGE_NAME}").push('latest')
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
