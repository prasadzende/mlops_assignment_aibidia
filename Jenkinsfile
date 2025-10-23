pipeline {
    agent {
        docker {
            image 'python:3.11-slim'
            args '-u root -v /var/run/docker.sock:/var/run/docker.sock'  // Run as root user
        }
    }

    triggers {
        pollSCM('* * * * *')  // Poll SCM every minute for changes
    }

    stages {
        stage('Install System Dependencies') {
            steps {
                sh '''
                    apt-get update
                    apt-get install -y make curl
                '''
            }
        }

        stage('Checkout') {
            steps {
                cleanWs()
                checkout scm
            }
        }

        stage('Setup Environment') {
            steps {
                dir('src/model_src') {
                    sh 'make setup'
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                dir('src/model_src') {
                    sh 'make install'
                }
            }
        }

        stage('Train Model') {
            steps {
                dir('src/model_src') {
                    sh 'make train'
                }
            }
        }

        stage('Cleanup') {
            steps {
                dir('src/model_src') {
                    sh 'make clean'
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}