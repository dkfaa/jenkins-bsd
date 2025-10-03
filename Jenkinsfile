pipeline {
    agent {
        node {
            label "linux-194 || linux-192"
        }
    }

    stages {
        stage('Name') {
            steps {
                echo 'Dika Aditya Putra'
            }
        }
    }

    post {
        always {
            echo "I will always say Hello Dika"
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
