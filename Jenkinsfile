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
        sh 'docker build -t raju925/biriyani-house:latest .'

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
        sh 'docker push raju925/biriyani-house:latest'
          }
        }

       stage('Deploy to Docker Swarm') {
            steps {
                sh """
                    docker service rm biriyani-house || true
                    docker service create --name biriyani-house -p 8070:80 ${Raju}:${TAG}
                """
                }
            }
        }
        
    post {
        always {
            echo 'Pipeline finished.'
        }
    }
}

