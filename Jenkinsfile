pipeline {
    agent any

    stages {
        /*
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
                    ls -la
                    node --version
                    npm --version
                    echo 'Installing dependencies...'
                    npm ci
                    npm run build
                    ls -la                     
                '''
            }            
        }
        
        stage("Test") {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                    args '-v /root/.m2:/root/.m2'
                }
            }

            steps {
                sh '''
                    echo "Test State - Start Testing"
                    # echo "=========================="
                    # test -f build/index.html
                    npm test
                '''
            }
        }
        */

        stage("E2E Test") {
            agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.39.0-noble'                    
                    reuseNode true
                    args '-v /root/.m2:/root/.m2'
                }
            }

            steps {
                sh '''
                    echo "E2E Test Starting"
                    npm install serve
                    node_modules/.bin/serve -s build &
                    sleep 10
                    npx playwright test
                '''
            }
        }

    }

    post {
        always {
            junit 'test-results/junit.xml'
        }
    }
}
