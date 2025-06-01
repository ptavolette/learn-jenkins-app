pipeline {
    agent any
    /* agent {
        docker {
            image 'node:18-alpine'
            reuseNode true
        }
    } */

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    echo "Hello from Jenkins"
                    whoami
                    ls -al
                    node --version
                    npm --version
                    npm ci
                    npm run build
                    ls -al
                '''
            }
        }
        stage('Test') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    echo "Test stage"
                    (ls build/index.html >> /dev/null 2>&1 && echo "yes") || echo "no"
                    test -f build/index.html
                    npm test
                '''
            }
        }
        stage('E2E') {
            agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.39.0-jammy'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    npm install - serve
                    # Need to provide path for serve binary
                    # And need & at end to run server in background and not block subsequent steps
                    node_modules/.bin/serve -s build &
                    sleep 10
                    npx playwright test --reporter:html
                '''
            }

        }
    }

    post {
        always {
            junit 'junit-test-results/junit.xml'
            publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, icon: '', keepAll: false, reportDir: 'playwright-report', reportFiles: 'index.html', reportName: 'Playwright HTML Report', reportTitles: '', useWrapperFileDirectly: true])
        }
    }
}