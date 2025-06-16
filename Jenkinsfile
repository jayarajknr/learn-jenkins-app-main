pipeline {
    agent any

    parameters {
        string(name: 'ROLLBACK_VERSION', defaultValue: '', description: 'Git tag or commit to roll back to')
    }

    environment {
        NETLIFY_SITE_ID = 'ad3e0d66-e12c-4a56-89f1-ab3007291ec6'
        NETLIFY_AUTH_TOKEN = credentials('netlify-token')
    }

    stages {

        stage('Create Docker Container ') {
            steps {
                sh 'docker build --no-cache -t my-playwright .'

            }
        }
        
/*        
        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                    args '-v /root/.m2:/root/.m2'
                }
            }
            steps {

                sh '''
                    cleanWs()
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
                
                stage("E2E Test - Local") {
                    agent {
                        docker {
                            image 'my-playwright'
                            reuseNode true
                        }
                    }

                    steps {
                        sh '''
                            echo "E2E - LOCAL - Test Starting"
                            npm install serve
                            node_modules/.bin/serve -s build &
                            sleep 10
                            npx playwright test --reporter=html
                        '''
                    }

                    post {
                        always {
                            publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, icon: '', keepAll: false, reportDir: 'playwright-report', reportFiles: 'index.html', reportName: 'Playwrite HTML Report - LOCAL', reportTitles: 'E2E Test Result', useWrapperFileDirectly: true])
                        }
                    }
                }
            }
        }
               
        stage('Staging - Deployment') {
            agent {
                docker {
                    image 'node:18'
                    reuseNode true
                }
            }
            steps {
                echo "Staging Deployment Started...!!!"
                sh '''
                   netlify --version 
                   echo "Deploying to Staging. Site ID : $NETLIFY_SITE_ID"   
                   netlify status
                   netlify deploy --dir=build --json > staging_deploy_output.json
                   #node_modules/.bin/node-jq -r '.deploy_url' staging_deploy_output.json
                '''
                script {
                    env.STAGING_URL = sh(script:"node_modules/.bin/node-jq -r '.deploy_url' staging_deploy_output.json", returnStdout:true)
                }
            }                        
        }

        stage("E2E Test - Staging") {
            environment {
                CI_ENVIRONMENT_URL = "${env.STAGING_URL}"
            }

            agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.39.0-focal'
                    reuseNode true
                }
            }

            steps {
                sh '''
                    echo "E2E - Staging Test Starting"
                    #npx playwright test --reporter=html
                '''
            }

            post {
                always {
                    publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, icon: '', keepAll: false, reportDir: 'playwright-report', reportFiles: 'index.html', reportName: 'Playwrite HTML Report - Staging', reportTitles: 'E2E Test Result', useWrapperFileDirectly: true])
                }
            }
        }
/*
        stage('Approval') {
            steps {
                timeout(5) {
                    input message: 'Approval Needed for PROD deployment. Do you want to Deploy?', ok: 'I Approve PROD Deployment'
                }
            }
        }
*/

/*
        stage('Production - Deployment') {
            agent {
                docker {
                    image 'node:18'
                    reuseNode true
                }
            } 
            steps {
                echo "Production Deployment Started...!!!"
                sh '''
                   npm install netlify-cli 
                   node_modules/.bin/netlify --version 
                   echo "Deploying to PRODUCTION. Site ID : $NETLIFY_SITE_ID"   
                   node_modules/.bin/netlify status
                   node_modules/.bin/netlify deploy --dir=build --prod
                '''
            }            
        }

        stage("E2E Test - PROD") {
            environment {
                CI_ENVIRONMENT_URL = "https://hilarious-croquembouche-b394da.netlify.app"
            }

            agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.39.0-focal'
                    reuseNode true
                }
            }

            steps {
                sh '''
                    echo "E2E - PRODUCTION Test Starting"
                    #npx playwright test --reporter=html
                '''
            }

            post {
                always {
                    publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, icon: '', keepAll: false, reportDir: 'playwright-report', reportFiles: 'index.html', reportName: 'Playwrite HTML Report - PRODUCTION', reportTitles: 'E2E Test Result', useWrapperFileDirectly: true])
                }
            }
        }
  */     
    }

    post {
        always {
            echo "DONE !!!!!!!!!!!!!!!!!!!!!!!!"
            /*
            junit 'jest-results/junit.xml'
            publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, icon: '', keepAll: false, reportDir: 'playwright-report', reportFiles: 'index.html', reportName: 'Playwrite HTML Report', reportTitles: 'E2E Test Result', useWrapperFileDirectly: true])
            */
        }
    }
    
}
