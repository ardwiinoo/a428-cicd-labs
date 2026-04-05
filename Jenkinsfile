pipeline {
    agent {
        docker {
            image 'node:16-buster-slim'
        }
    }
    stages {
        stage('Clean') {
            steps {
                deleteDir()
            }
        }
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build') {
            steps {
                sh 'npm ci --legacy-peer-deps'
            }
        }
        stage('Test') {
            steps {
                sh './jenkins/scripts/test.sh'
            }
        }
    }
}