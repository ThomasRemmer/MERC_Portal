pipeline {
    agent any

    stages {
        stage('Build') {
            agent {
                dockerContainer {
                    image node:18-alpine
                }
            }
            steps {
                sh '''
                    ls -la
                '''
            }
        }
    }
}
