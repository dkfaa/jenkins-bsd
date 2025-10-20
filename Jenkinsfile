pipeline {
    agent {
        node {
            label "linux-132 || linux-192"
        }
    }

    stages {
        stage('Build') {
            steps {
                script {
                    for (int i = 0; i < 10; i++){
                        echo("Script ${i}")
                    }
                }
                echo 'Start Build'
                sh("./mvnw clean compile test-compile")
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
                sh("./mvnw test")
                echo 'Finish Test'
            }
        }

    }

    post {
        always {
            echo "I will always say Use Full IT"
        }

        success {
            echo  "Yay, success"
        }

        failure {
            echo "Oh no, failure"
        }

        cleanup {
            echo "Don't care success or error"
        }
    }
}
