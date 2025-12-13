pipeline {
    agent any

    stages {
        stage('add docker images') {
            steps {
                sh '''
                docker build -t rnxg-3d:latest .
                docker images | grep rnxg-3d
                '''
            }
        }

        stage('run docker images') {
            steps {
                sh '''
                docker rm -f rnxg-3d || true
                docker run -d -p 8083:80 --name rnxg-3d rnxg-3d:latest
                '''
            }
        }
    }
}
