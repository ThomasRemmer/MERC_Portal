pipeline {
    agent any 


    stages {
        agent {
        docker { image 'node:22.14.0-alpine3.21'}
        resuseNode true
    }
        stage('Build') { 
            steps {
                sh 'node --version'
                sh 'npm install' 
            }
        }
    }
}