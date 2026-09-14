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
    }
}