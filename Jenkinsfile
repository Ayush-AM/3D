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
        HELM_RELEASE = "rnxg3d"
        HELM_CHART = "./rnxg-3d"
        KUBE_NAMESPACE = "default"
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
                '''
            }
        }

        stage('deploy using helm'){
            steps{
                script{
                    sh '''
                    helm upgrade --install ${HELM_RELEASE} ${HELM_CHART} \
                    --namespace ${KUBE_NAMESPACE} \
                    --set image.repository=${DOCKER_IMAGE} \
                    --set image.tag=${DOCKER_TAG}
                    --kube-insecure-skip-tls-verify
                    '''
                }
            }
        }

        stage('Verify Deployment'){
            steps{
                sh '''
                kubectl get pods
                kubectl get svc
                '''
            }
        }
    }

    post {
        success {
            echo "✅ Deployment Successful"
        }
        failure {
            echo "❌ Deployment Failed"
        }
    }
}
