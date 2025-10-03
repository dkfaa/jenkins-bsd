pipeline {
    agent {
        node {
            label "linux-194 || linux-192"
        }
    }

    stages {
        stage('Build') {
            steps {
                echo 'Start Build'
                sh("./mvnw clean compile test-compile")
                echo 'Finish Build'
            }
        }

        stage('Test') {
            steps {
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
