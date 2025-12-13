pipeline {
    agent any

    stages {
        stage('add docker images') {
            steps {
                echo "docker build -t rnxg-3d ."
            }
        }

        stage('run docker images') {
            steps {
                sh '''
                docker run -d -p 8083:80 --name rnxg-3d rnxg-3d
                '''
            }
        }
    }
}
