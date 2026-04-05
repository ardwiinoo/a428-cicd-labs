pipeline {
    agent any

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
            agent {
                docker {
                    image 'node:16-buster-slim'
                    reuseNode true
                }
            }
            steps {
                sh 'npm install --legacy-peer-deps'
            }
        }

        stage('Test') {
            agent {
                docker {
                    image 'node:16-buster-slim'
                    reuseNode true
                }
            }
            steps {
                sh 'chmod +x ./jenkins/scripts/test.sh'
                sh './jenkins/scripts/test.sh'
            }
        }
    }
}