pipeline {
    agent any

    stages {
        stage('Get Global Build Number') {
            steps {
                script {
                    // Trigger the counter job
                    def counterBuild = build job: 'global-build-counter', wait: true, propagate: true

                    // Use its build number
                    env.GLOBAL_BUILD_NUMBER = counterBuild.getNumber().toString()

                    // Show it in Jenkins UI
                    currentBuild.displayName = "#${env.GLOBAL_BUILD_NUMBER}"
                }
            }
        }

        stage('Checkout') {
            steps {
                echo "Checking out the repository..."
                // Normally: git url: 'https://github.com/username/repo.git', credentialsId: 'your-credentials'
            }
        }

        stage('Build') {
            steps {
                echo "Building the project with Global Build Number: ${env.GLOBAL_BUILD_NUMBER}"
                // Example build command: sh 'mvn clean package'
            }
        }

        stage('Test') {
            steps {
                echo "Running tests..."
                // Example test command: sh 'mvn test'
            }
        }

        stage('Deploy') {
            steps {
                echo "Deploying the application..."
                // Example deploy command: sh './deploy.sh'
            }
        }

        stage('Cleanup') {
            steps {
                echo "Cleaning up workspace..."
                // Example cleanup: deleteDir()
            }
        }
    }

    post {
        success {
            echo "Pipeline completed successfully! Build #${env.GLOBAL_BUILD_NUMBER}"
        }
        failure {
            echo "Pipeline failed! Build #${env.GLOBAL_BUILD_NUMBER}"
        }
        always {
            echo "This always runs regardless of success or failure."
        }
    }
}
