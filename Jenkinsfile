pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo 'This is a test checkout stage'
                // Normally you'd clone a repo here
                // git url: 'REPO_URL', credentialsId: 'CREDENTIALS_ID'
            }
        }

        stage('Build') {
            steps {
                echo 'This is a test build stage'
                // Normally you'd build your project here
            }
        }

        stage('Test') {
            steps {
                echo 'This is a test stage'
                // Normally you'd run tests here
            }
        }

        stage('Deploy') {
            steps {
                echo 'This is a test deploy stage'
                // Normally you'd deploy your app here
            }
        }
    }

    post {
        always {
            echo 'This is a test pipeline run'
        }
    }
}
