pipeline {
    agent any

    environment {
        IMAGE_NAME = "raju925/biriyani-house"
        TAG = "latest"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git url: 'https://github.com/Raju-227/new-project.git', branch: 'main'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("${IMAGE_NAME}:${TAG}")
                }
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh 'echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    dockerImage.push("${TAG}")
                }
            }
        }

        stage('Deploy to Docker Swarm') {
            steps {
                script {
                    sh """
                    docker service rm biriyani-house || true
                    docker service create \
                      --name biriyani-house \
                      --publish 8070:80 \
                      ${IMAGE_NAME}:${TAG}
                    """
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished.'
        }
    }
}

