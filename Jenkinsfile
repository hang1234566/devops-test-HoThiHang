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
                sh 'npm install'
            }
        }

        stage('Build') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Deploy') {
            steps {
                sh 'npm run deploy'
            }
        }
    }

    post {
        success {
            echo 'DEPLOY SUCCESS'
        }

        failure {
            echo 'DEPLOY FAILED'
        }
    }
}
