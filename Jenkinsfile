pipeline {
    agent any

    environment{
        DOCKER_IMAGE = "ankush1808/rnxg-3d"
        DOCKER_CREDS = "dockerhub-creds"
    }

    stages {
        stage('build docker images') {
            steps {
                sh '''
                docker build -t $DOCKER_IMAGE:latest .
                docker images
                '''
            }
        }

        stage('login to docker hub'){
            steps{
                withCredentials([usernamePassword(
                    credentialsId: DOCKER_CREDS,
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                }
            }
        }

        stage('push docker images') {
            steps {
                sh '''
                docker push $DOCKER_IMAGE:latest
                '''
            }
        }
    }
}
