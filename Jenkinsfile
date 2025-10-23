pipeline {
    agent {
        node {
            label "linux-132 || linux-192"
        }
    }

    environment {
        DISCORD_WEBHOOK_URL = 'https://discord.com/api/webhooks/1430773000881311754/yMwbYbm7g5gGVUyqKIHxgvuF_Ozu-segTIL-sYpYF0yxOdg3bs9c654fl80nDmjLkogy' // ganti dengan webhook kamu
    }

    stages {
        stage('Build') {
            steps {
                echo 'Start Build'
                sh "./mnw clean compile test-compile"
                echo 'Finish Build'
            }
        }

        stage('Test') {
            steps {
                script {
                    def data = [
                        "firstname": "Dika",
                        "lastname": "Aditya"
                    ]
                    writeJSON(file: "data.json", json: data)
                }

                echo 'Start Test'
                sh "./mvnw test"
                echo 'Finish Test'
            }
        }
    }

    post {
        success {
            // Discord Notification
            script {
                sh """
                    curl -H "Content-Type: application/json" \
                    -X POST \
                    -d '{"content": "🟢 Jenkins Job SUCCESS: ${JOB_NAME} #${BUILD_NUMBER} berhasil 🚀"}' \
                    $DISCORD_WEBHOOK_URL
                """
            }
        }

        failure {
            // Discord Notification
            script {
                sh """
                    curl -H "Content-Type: application/json" \
                    -X POST \
                    -d '{"content": "🔴 Build FAILED: ${JOB_NAME} #${BUILD_NUMBER} — cek log untuk detail ⚠️"}' \
                    $DISCORD_WEBHOOK_URL
                """
            }
        }
    }
}