pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "prateek0017/trend-devops-app"
    }

    stages {

        stage('Clone Repository') {
            steps {
                git 'https://github.com/prats00017/trend-devops-project.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE:latest .'
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {

                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                sh 'docker push $DOCKER_IMAGE:latest'
            }
        }

        stage('Run Docker Container') {
            steps {
                sh '''
                docker stop trend-container || true
                docker rm trend-container || true
                docker run -d -p 80:80 --name trend-container $DOCKER_IMAGE:latest
                '''
            }
        }
    }
}
