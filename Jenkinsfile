pipeline {
    agent any

    environment {
        CLOUDFLARE_API_TOKEN = credentials('cloudflare-api-token')
        TELEGRAM_BOT_TOKEN = credentials('telegram-bot-token')
        TELEGRAM_CHAT_ID = credentials('telegram-chat-id')
    }

    stages {

        stage('Telegram Started') {
            steps {
                sh '''
                    curl -s -X POST "https://api.telegram.org/bot${TELEGRAM_BOT_TOKEN}/sendMessage" \
                    --data-urlencode "chat_id=${TELEGRAM_CHAT_ID}" \
                    --data-urlencode "text=🔵 STARTED - DevOps Ho Thi Hang%0A%0ABuild: #${BUILD_NUMBER}"
                '''
            }
        }

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
                sh 'chmod +x node_modules/.bin/wrangler'
                sh 'npm run deploy'
            }
        }
    }

    post {
        always {
            script {
                def status = currentBuild.currentResult

                if (status == 'SUCCESS') {
                    sh '''
                        curl -s -X POST "https://api.telegram.org/bot${TELEGRAM_BOT_TOKEN}/sendMessage" \
                        --data-urlencode "chat_id=${TELEGRAM_CHAT_ID}" \
                        --data-urlencode "text=🟢 SUCCESS - DevOps Ho Thi Hang%0A%0ABuild: #${BUILD_NUMBER}%0AWebsite: https://evops.ho-thi-hang1002.workers.dev"
                    '''
                } else {
                    sh '''
                        curl -s -X POST "https://api.telegram.org/bot${TELEGRAM_BOT_TOKEN}/sendMessage" \
                        --data-urlencode "chat_id=${TELEGRAM_CHAT_ID}" \
                        --data-urlencode "text=🔴 FAILED - DevOps Ho Thi Hang%0A%0ABuild: #${BUILD_NUMBER}%0AStatus: ${currentBuild.currentResult}"
                    '''
                }
            }
        }
    }
}