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
                sh """
                    curl -s -X POST "https://api.telegram.org/bot\${TELEGRAM_BOT_TOKEN}/sendMessage" \
                    --data-urlencode "chat_id=\${TELEGRAM_CHAT_ID}" \
                    --data-urlencode "text=🔴 FAILED - DevOps Ho Thi Hang%0A%0ABuild: #\${BUILD_NUMBER}%0AStatus: ${status}"
                """
            }
        }
    }
}