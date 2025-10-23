pipeline {
    agent {
        docker {
            image 'python:3.11-slim'
            args '-u root'  // Run as root to have permissions for package installation
        }
    }

    environment {
        PATH = "/opt/homebrew/bin:${env.PATH}"
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
                    sh '''
                        python -m pip install --upgrade pip
                        python -m pip install pdm
                        make setup
                    '''
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