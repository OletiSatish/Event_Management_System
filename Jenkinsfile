pipeline {
    agent any

    environment {
        DOCKER_USER = "satishosk" 
        IMAGE_NAME = "simple-html-website"
        IMAGE_TAG = "latest"
        CONTAINER_NAME = "html-website"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'master', url: 'https://github.com/OletiSatish/Event_Management_System.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${DOCKER_USER}/${IMAGE_NAME}:${IMAGE_TAG} ."
                }
            }
        }

        stage('Push Image to Docker Hub') {
            steps {
                script {
                    withDockerRegistry([credentialsId: 'Docker_Cred', url: '']) {
                        sh "docker push ${DOCKER_USER}/${IMAGE_NAME}:${IMAGE_TAG}"
                    }
                }
            }
        }

        stage('Deploy Container') {
            steps {
                script {
                    // Stop and remove existing container (if running)
                    sh "docker stop ${CONTAINER_NAME} || true"
                    sh "docker rm ${CONTAINER_NAME} || true"

                    // Remove old images (Optional)
                    sh "docker image prune -f"

                    // Run the new container
                    sh "docker run -d --name ${CONTAINER_NAME} -p 80:80 ${DOCKER_USER}/${IMAGE_NAME}:${IMAGE_TAG}"
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
