pipeline {

    agent any

    environment {
        IMAGE_NAME = "shelarrahul1415/react-app"
        IMAGE_TAG = "latest"
    }

    stages {
        stage('Checkout') {
            steps {

                git branch: 'main',
                    url: 'https://github.com/rahulshelar14/testing-jenking-react-docker-k8s.git'
            }
        }

        stage('Build Docker Image') {
            steps {

                sh '''
                    docker build -t $IMAGE_NAME:$IMAGE_TAG .
                '''
            }
        }

        stage('Docker Login') {

            steps {

                withCredentials([
                    usernamePassword(
                        credentialsId: 'docker-hub-cred',
                        usernameVariable: 'DOCKER_USERNAME',
                        passwordVariable: 'DOCKER_PASSWORD'
                    )
                ]) {

                    sh '''
                        echo "$DOCKER_PASSWORD" | docker login \
                        -u "$DOCKER_USERNAME" \
                        --password-stdin
                    '''
                }
            }
        }

        stage('Push Docker Image') {
            steps {

                sh '''
                    docker push $IMAGE_NAME:$IMAGE_TAG
                '''
            }
        }
    }
}