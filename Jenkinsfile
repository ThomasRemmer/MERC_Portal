pipeline {
    stages {
        stage('Build') {
            agent {
                any {
                    image node:18-alpine
                    reuseNode true
                }
            }
            steps {
                sh '''
                    ls -la
                    node --version
                    npm --version
                    npm ci
                    npm run build
                '''
            }
        }
    }
}
