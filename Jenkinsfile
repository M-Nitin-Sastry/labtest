pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "nitinsastry31/nitinapp"
        DOCKER_TAG = "latest"
        CONTAINER_NAME = "myapp-container"
    }

    stages {

        stage('Clone Code') {
            steps {
                git 'https://github.com/M-Nitin-Sastry/labtest.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE:$DOCKER_TAG .'
            }
        }

        stage('Login to DockerHub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-credentials',
                    usernameVariable: 'USER',
                    passwordVariable: 'PASS'
                )]) {
                    sh 'echo $PASS | docker login -u $USER --password-stdin'
                }
            }
        }

        stage('Push Image') {
            steps {
                sh 'docker push $DOCKER_IMAGE:$DOCKER_TAG'
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                docker stop $CONTAINER_NAME || true
                docker rm $CONTAINER_NAME || true
                docker run -d -p 5000:5000 \
                --name $CONTAINER_NAME \
                $DOCKER_IMAGE:$DOCKER_TAG
                '''
            }
        }
    }
}
