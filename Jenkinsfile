pipeline {
    agent any

    triggers {
        pollSCM('* * * * *')  // Poll SCM every minute for changes
    }

    stages {
        // stage('Install System Dependencies') {
        //     steps {
        //         sh '''
        //             sudo apt-get -y update
        //             sudo apt-get install -y make curl python3 python3-pip
        //             python3 -m pip install --user --upgrade pip
        //         '''
        //     }
        // }

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