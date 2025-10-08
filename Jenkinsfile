pipeline {
    agent any

    environment {
        IMAGE_NAME = "raju925/insta"
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
        sh 'docker build -t raju925/insta:latest .'

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
        sh 'docker push raju925/insta:latest'
          }
        }

       stage('Deploy to Docker Swarm') {
            steps {
                sh """
            ssh raju@manager-node 'docker stack deploy -c docker-compose.yml insta'
                    docker service create --name jcon -p 8081:80 ${Raju}:${TAG}
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

