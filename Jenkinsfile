pipeline {
    agent any

    environment {
        IMAGE_NAME = "satishosk/html-webpage"
        CONTAINER_NAME = "html-webpage-container"
        GITHUB_REPO = "https://github.com/OletiSatish/Event_Management_System.git"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'master', url: "${GITHUB_REPO}"
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${IMAGE_NAME}:latest ."
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    withDockerRegistry([credentialsId: 'Docker_Cred', url: '']) {
                        sh "docker push ${IMAGE_NAME}:latest"
                    }
                }
            }
        }

        stage('Deploy Container') {
            steps {
                script {
                    sh "docker stop ${CONTAINER_NAME} || true"
                    sh "docker rm ${CONTAINER_NAME} || true"
                    sh "docker run -d --name ${CONTAINER_NAME} -p 80:80 ${IMAGE_NAME}:latest"
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Deployment failed!'
        }
    }
}
