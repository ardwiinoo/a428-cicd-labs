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
                sh '''
                    rm -rf node_modules package-lock.json
                    npm cache clean --force
                    npm install --legacy-peer-deps
                '''
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

        stage('Manual Approval') {
            steps {
                input message: 'Lanjutkan ke tahap Deploy?', ok: 'Proceed'
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    docker rm -f react-app-deploy || true
                    docker run -d \
                    --name react-app-deploy \
                    -p 3001:3000 \
                    -v "$WORKSPACE":/app \
                    -w /app \
                    -e HOST=0.0.0.0 \
                    node:16-buster-slim \
                    sh -c "npm start"

                    sleep 60

                    docker stop react-app-deploy || true
                    docker rm -f react-app-deploy || true
                '''
            }
        }
    }
}