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
            // Discord Notifications
            script {
                sh """
                curl -H "Content-Type: application/json" \
                -X POST -d '{"content: "✅ Jenkins Job *SUCCESS*: ${env.JOB_NAME} #${env.BUILD_NUMBER}"
                $DISCORD_WEBHOOK_URL}
                """
            }
        }
    }
}