pipeline {
    agent any

    stages {

        stage('Generate Global Build Number') {
            steps {
                script {
                    lock('global-build-number-lock') {  // ensures only one build increments at a time

                        def counterFile = "${JENKINS_HOME}/global-build-number.txt"

                        // Initialize counter if missing
                        if (!fileExists(counterFile)) {
                            writeFile file: counterFile, text: "1"
                            echo "Counter file created with initial value 1"
                        }

                        // Read current number
                        def buildNumber = readFile(counterFile).trim().toInteger()

                        // Increment for next build
                        def nextNumber = buildNumber + 1
                        writeFile file: counterFile, text: nextNumber.toString()

                        // Set environment variables
                        env.GLOBAL_BUILD_NUMBER = buildNumber.toString()
                        env.RELEASE_VERSION = "1.0.${env.GLOBAL_BUILD_NUMBER}"
                        env.IMAGE_TAG = "${env.BRANCH_NAME.replaceAll('/', '-')}-${env.GLOBAL_BUILD_NUMBER}"

                        currentBuild.displayName = "#${env.GLOBAL_BUILD_NUMBER}"
                        currentBuild.description = "Branch: ${env.BRANCH_NAME}"

                        echo "Global Build Number: ${env.GLOBAL_BUILD_NUMBER}"
                        echo "Release Version: ${env.RELEASE_VERSION}"
                        echo "Docker Image Tag: ${env.IMAGE_TAG}"
                    }
                }
            }
        }

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh 'echo "Would run: mvn clean compile"'
            }
        }

        stage('Unit Tests') {
            steps {
                sh 'echo "Would run: mvn test"'
            }
            post {
                always {
                    echo "Would parse junit reports here"
                }
            }
        }

        stage('Static Analysis') {
            steps {
                sh 'echo "Would run: mvn sonar:sonar"'
            }
        }

        stage('Package') {
            steps {
                sh "echo 'Would run: mvn package -Drevision=${env.RELEASE_VERSION}'"
            }
        }

        stage('Build Docker Image') {
            when { branch 'main' }
            steps {
                sh "echo 'Would run: docker build -t myapp:${env.IMAGE_TAG} .'"
            }
        }

        stage('Deploy') {
            when { branch 'main' }
            steps {
                sh "echo 'Would run: ./deploy.sh ${env.RELEASE_VERSION}'"
            }
        }
    }

    post {
        success {
            echo "Build ${env.GLOBAL_BUILD_NUMBER} (${env.BRANCH_NAME}) completed successfully!"
        }
        failure {
            echo "Build ${env.GLOBAL_BUILD_NUMBER} (${env.BRANCH_NAME}) failed!"
        }
        always {
            cleanWs()
        }
    }
}
