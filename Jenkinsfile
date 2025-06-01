pipeline {
    agent any

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
            steps {
                sh '''
                    echo "Test stage"
                    (ls build/index.html >> /dev/null 2>&1 && echo "yes") || echo "no"
                    npm test
                '''
            }
        }
    }
}