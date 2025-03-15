pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building project...'
                sh 'sleep 5'  
            }
        }
    }

    post {
        success {
            script {
                sendTelegramMessage("✅ Jenkins: Build успішно завершено!")
            }
        }
        failure {
            script {
                sendTelegramMessage("❌ Jenkins: Build провалено!")
            }
        }
    }
}


def sendTelegramMessage(String message) {
    def botToken = "7395725902:AAH_z4OursKRB6qzKb1DOwKsSlDQcthh1dQ"
    def chatId = "1046935503"
    def encodedMessage = URLEncoder.encode(message, "UTF-8")
    sh "curl -s -X POST https://api.telegram.org/bot${botToken}/sendMessage -d chat_id=${chatId} -d text=${encodedMessage}"
}


