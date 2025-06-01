pipeline {
    agent any

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:alpine18'
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
    }
}