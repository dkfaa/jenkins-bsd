pipeline {
    agent {
        node {
            label "linux-132 || linux-192"
        }
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
            script {
                withCredentials([string(credentialsId: 'discord-webhook', variable: 'DISCORD_WEBHOOK_URL')]) {
                    sh '''#!/bin/bash
                    PAYLOAD=$(cat <<EOF
    {
    "content": "✅ Jenkins Job *SUCCESS*: ${JOB_NAME} #${BUILD_NUMBER}"
    }
    EOF
    )
                    curl -H "Content-Type: application/json" \
                        -X POST \
                        -d "$PAYLOAD" \
                        "$DISCORD_WEBHOOK_URL"
                    '''
                }
            }
        }
    }
}