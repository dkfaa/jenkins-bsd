pipeline {
    agent {
        node {
            label "linux-132 || linux-192"
        }
    }

    environment {
        // Ambil dari Jenkins Credentials (Secret Text)
        DISCORD_WEBHOOK_URL = credentials('discord-webhook')
    }

    stages {
        stage('Build') {
            steps {
                script {
                    def payload = [
                        embeds: [[
                            title: "🚀 Build Started",
                            description: "Job `${env.JOB_NAME}` build #${env.BUILD_NUMBER} has started.\n<${env.BUILD_URL}>",
                            color: 3447003
                        ]]
                    ]

                    // Tulis ke file JSON
                    writeJSON file: 'payload.json', json: payload, pretty: 4

                    // Kirim ke Discord via curl
                    sh """
                    curl -H "Content-Type: application/json" \
                         -X POST \
                         -d @payload.json \
                         "${env.DISCORD_WEBHOOK_URL}"
                    """
                }

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
        always {
            echo "I will always say Use Full IT"
        }

        success {
            echo "Yay, success"
            script {
                sendDiscord("✅ Build Success", "Job `${env.JOB_NAME}` build #${env.BUILD_NUMBER}` succeeded ✅\n<${env.BUILD_URL}>")
            }
        }

        failure {
            echo "Oh no, failure"
            script {
                sendDiscord("❌ Build Failed", "Job `${env.JOB_NAME}` build #${env.BUILD_NUMBER}` failed ❌\n<${env.BUILD_URL}>")
            }
        }

        unstable {
            echo "Build marked as unstable"
            script {
                sendDiscord("⚠️ Build Unstable", "Job `${env.JOB_NAME}` build #${env.BUILD_NUMBER}` is unstable ⚠️\n<${env.BUILD_URL}>")
            }
        }

        aborted {
            echo "Build aborted"
            script {
                sendDiscord("🟡 Build Aborted", "Job `${env.JOB_NAME}` build #${env.BUILD_NUMBER}` was aborted 🟡\n<${env.BUILD_URL}>")
            }
        }

        notBuilt {
            echo "Build not built"
            script {
                sendDiscord("🧱 Build Not Built", "Job `${env.JOB_NAME}` build #${env.BUILD_NUMBER}` was not built 🧱\n<${env.BUILD_URL}>")
            }
        }

        regression {
            echo "Build regression detected"
            script {
                sendDiscord("📉 Build Regression", "Job `${env.JOB_NAME}` build #${env.BUILD_NUMBER}` indicates regression 📉\n<${env.BUILD_URL}>")
            }
        }

        changed {
            echo "Build state changed (Back To Normal)"
            script {
                sendDiscord("🔁 Build Back To Normal", "Job `${env.JOB_NAME}` build #${env.BUILD_NUMBER}` is back to normal 🔁\n<${env.BUILD_URL}>")
            }
        }

        cleanup {
            echo "Don't care success or error"
        }
    }
}

// === Discord Notification Function ===
def sendDiscord(title, message) {
    def color = 0
    if (title.contains("Success")) {
        color = 3066993   // Hijau
    } else if (title.contains("Failed")) {
        color = 15158332  // Merah
    } else if (title.contains("Unstable")) {
        color = 15105570  // Kuning
    } else if (title.contains("Aborted")) {
        color = 10197915  // Abu-abu
    } else {
        color = 3447003   // Biru (default)
    }

    def payload = [
        embeds: [[
            title: title,
            description: message,
            color: color
        ]]
    ]

    // Tulis ke JSON file sementara
    writeJSON file: 'payload_post.json', json: payload, pretty: 4
    def jsonText = readFile('payload_post.json')

    sh """
    curl -H "Content-Type: application/json"
