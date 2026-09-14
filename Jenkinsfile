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
                sh '''
                    python3 -m venv venv
                    ./venv/bin/pip install -r requirements.txt
                '''
            }
        }

        stage('Test') {
            steps {
                sh './venv/bin/python manage.py test'
            }
        }

        stage('Deploy') {
            steps {
                sshagent(['deployment-ec2-key']) {
                    sh '''
                        tar --exclude='.git' --exclude='venv' -czf portfolio.tar.gz .

                        scp -o StrictHostKeyChecking=no portfolio.tar.gz ubuntu@3.110.188.17:/home/ubuntu/portfolio.tar.gz

                        ssh -o StrictHostKeyChecking=no ubuntu@3.110.188.17 "
                            rm -rf /home/ubuntu/portfolio/*
                            tar -xzf /home/ubuntu/portfolio.tar.gz -C /home/ubuntu/portfolio
                            rm -f /home/ubuntu/portfolio.tar.gz
                        "

                        rm -f portfolio.tar.gz
                    '''
                }
            }
        }
    }
}