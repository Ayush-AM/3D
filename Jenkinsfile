pipeline {
    agent any

    // using docker-compose file
    // stages{
    //     stage('using docker-compose file'){
    //         steps{
    //             sh '''
    //             docker-compose up -d
    //             docker images
    //             '''
    //         }
    //     }
    // }

    //build and push docker image to docker hub
    environment{
        DOCKER_IMAGE = "ankush1808/rnxg-3d"
        DOCKER_CREDS = "dockerhub-creds"
        DOCKER_TAG = "${BUILD_NUMBER}"
    }

    stages {
        stage('build docker images') {
            steps {
                sh '''
                docker build -t $DOCKER_IMAGE:$DOCKER_TAG .
                docker tag $DOCKER_IMAGE:$DOCKER_TAG $DOCKER_IMAGE:latest
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
                docker push $DOCKER_IMAGE:$DOCKER_TAG
                docker push $DOCKER_IMAGE:latest
                '''
            }
        }

        stage('deploy to kubernetes cluster'){
            steps{
                sh '''
                kubectl apply -f deployment-rnxg3d.yaml
                kubectl apply -f service-rnxg3d.yaml
                kubectl get pods
                kubectl get services
                '''
            }
        }
    }
}
