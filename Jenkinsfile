pipeline {
    agent any

    environment {
        NETLIFY_SITE_ID = 'ad3e0d66-e12c-4a56-89f1-ab3007291ec6'
        NETLIFY_AUTH_TOKEN = credentials('netlify-token')
    }

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
                //cleanWs()

                sh '''
                    cleanWs()
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
        
        stage("Tests") {
            parallel {
                stage("Unit Test") {
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
                
                stage("E2E Test") {
                    agent {
                        docker {
                            //image 'mcr.microsoft.com/playwright:v1.39.0-noble'
                            image 'mcr.microsoft.com/playwright:v1.39.0-focal'
                            reuseNode true
                            //args '-v /root/.m2:/root/.m2'
                        }
                    }

                    steps {
                        sh '''
                            echo "E2E Test Starting"
                            npm install serve
                            node_modules/.bin/serve -s build &
                            sleep 10
                            npx playwright test --reporter=html
                        '''
                    }
                }
            }
        }

        stage('Deploy') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                    //args '-v /root/.m2:/root/.m2'
                }
            }
            steps {
                echo "Deployment Started...!"
                sh '''
                   npm install netlify-cli 
                   node_modules/.bin/netlify --version 
                   echo "Deploying to PRODUCTION. Site ID : $NETLIFY_SITE_ID"   
                   node_modules/.bin/netlify status
                  # node_modules/.bin/netlify deploy --dir=build --prod
                '''
            }            
        }
    }

    /*post {
        always {
            junit 'jest-results/junit.xml'
            publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, icon: '', keepAll: false, reportDir: 'playwright-report', reportFiles: 'index.html', reportName: 'Playwrite HTML Report', reportTitles: 'E2E Test Result', useWrapperFileDirectly: true])
        }
    }
    */
}
