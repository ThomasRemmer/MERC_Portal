pipeline {
    agent any
    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:22.14.0-alpine3.21'

                    reuseNode true
                }
            }
            steps {
                sh 'node --version'
            }
        }
    }
}