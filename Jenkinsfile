pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'python3 -m pip install -r requirements.txt'
            }
        }

        stage('Test') {
            steps {
                sh 'python3 manage.py test'
            }
        }

        stage('Deploy') {
            steps {
                sshagent(['deployment-ec2-key']) {
                    sh '''
                        ssh -o StrictHostKeyChecking=no ubuntu@3.110.188.17 "
                            rm -rf /home/ubuntu/portfolio/*
                        "

                        scp -o StrictHostKeyChecking=no -r ./* ubuntu@3.110.188.17:/home/ubuntu/portfolio/
                    '''
                }
            }
        }
    }
}