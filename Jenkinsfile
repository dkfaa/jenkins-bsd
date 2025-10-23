pipeline {
    agent {
        node {
            label "linux-132 || linux-192"
        }
    }

    environment {
        DISCORD_WEBHOOK_URL = credentials('discord-webhook') // ganti dengan webhook kamu
    }

    stages {
        stage('Build') {
            steps {
                // Discord Notification
                sh """
                    curl -H "Content-Type: application/json" \
                    -X POST \
                    -d '{"content": "🟡 Build STARTED: ${JOB_NAME} #${BUILD_NUMBER} 🚧"}' \
                    $DISCORD_WEBHOOK_URL
                """
                echo 'Start Build'
                sh "./mvnw clean compile test-compile"
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
                -d '{
                "embeds": [{
                    "title": "🟢 Jenkins Build SUCCESS",
                    "description": "Job **${JOB_NAME}** build #**${BUILD_NUMBER}** berhasil dijalankan! 🚀\\nLihat detail di [Build Logs](${BUILD_URL}).",
                    "color": 3066993,
                    "footer": {
                    "text": "© 2025 QA Jenkins CI/CD"
                    },
                    "thumbnail": {
                    "url": "https://www.jenkins.io/images/logos/jenkins/jenkins.png"
                    }
                }]
                }' \
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
                -d '{
                "embeds": [{
                    "title": "🔴 Jenkins Build FAILED",
                    "description": "Job **${JOB_NAME}** build #**${BUILD_NUMBER}** mengalami error⚠️ \\nLihat detail di [Build Logs](${BUILD_URL}).",
                    "color": 3066993,
                    "footer": {
                    "text": "© 2025 QA Jenkins CI/CD"
                    },
                    "thumbnail": {
                    "url": "https://www.jenkins.io/images/logos/jenkins/jenkins.png"
                    }
                }]
                }' \
                $DISCORD_WEBHOOK_URL
                """


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
