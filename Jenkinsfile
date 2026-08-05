pipeline {
    agent {
        label 'docker-agent'
    }

    triggers {
        // Runs every 2 minutes for the first 15 minutes of every hour
        cron('H(0-14)/2 * * * *')
    }

    environment {
        IMAGE_NAME = 'pallavamsidhar24/my-web-app'
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                dir('docker-practice') {
                    sh 'docker build -t $IMAGE_NAME:latest -t $IMAGE_NAME:$BUILD_NUMBER .'
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                    sh 'docker push $IMAGE_NAME:latest'
                    sh 'docker push $IMAGE_NAME:$BUILD_NUMBER'
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
