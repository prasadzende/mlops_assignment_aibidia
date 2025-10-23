pipeline {
    agent any

    triggers {
        pollSCM('* * * * *')  // Poll SCM every minute for changes
    }

    stages {
        stage('Checkout') {
            steps {
                // Clean workspace before checkout
                cleanWs()
                // Checkout code from Git repository
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
            cleanWs()  // Clean workspace after build
        }
    }
}