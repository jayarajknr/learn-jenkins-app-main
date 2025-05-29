pipeline {
    agent any

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                    args '-v /root/.m2:/root/.m2'
                    //args '-v /var/run/docker.sock:/var/run/docker.sock'
                }
            }
            steps {
                sh '''
                    echo 'Hello Jenkins!!!'
                    echo 'Building the project...'
                    node --version
                '''
            }
        }
    }
}
