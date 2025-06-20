pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = 'dockerhub-creds-id'  // Jenkins credentials ID for Docker Hub
        IMAGE_NAME = 'dexter7371/frontend-angular-19' // Docker Hub image name
        APP_FOLDER = 'frontend-angular-19'            // Folder where Dockerfile is located
    }

stage('Checkout Code') {
    steps {
        git branch: 'main', url: 'https://github.com/dexter73710/Jenkinsfile.git'
    }
}


        stage('Build Docker Image') {
            steps {
                dir("${APP_FOLDER}") {
                    script {
                        docker.build("${IMAGE_NAME}", '.')
                    }
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
