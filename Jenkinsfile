pipeline {
    agent any

    stages {
    stage('Generate Global Build Number') {
      steps {
        script {
          lock(resource: 'global-build-number-lock') {

            def counterFile = "${JENKINS_HOME}/global-build-number.txt"
            def buildNumber

            if (fileExists(counterFile)) {
              buildNumber = readFile(counterFile).trim().toInteger()
            } else {
              buildNumber = 1
            }

            env.GLOBAL_BUILD_NUMBER = buildNumber.toString()

            writeFile file: counterFile, text: (buildNumber + 1).toString()

            echo "Global Build Number: ${env.GLOBAL_BUILD_NUMBER}"
          }
        }
      }
    }
  }     

        stage('Checkout') {
            steps {
                echo "Checking out repository..."
                // git url: 'https://github.com/username/repo.git', credentialsId: 'github-credentials'
            }
        }

        stage('Build') {
            steps {
                echo "Building project with Global Build Number: ${env.GLOBAL_BUILD_NUMBER}"
                // sh 'mvn clean package'
            }
        }

        stage('Test') {
            steps {
                echo "Running tests..."
                // sh 'mvn test'
            }
        }

        stage('Deploy') {
            steps {
                echo "Deploying application..."
                // sh './deploy.sh'
            }
        }

        stage('Cleanup') {
            steps {
                echo "Cleaning up workspace..."
                // deleteDir()
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
