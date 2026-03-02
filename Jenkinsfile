pipeline {
    agent any

    stages {

        stage('Generate Global Build Number') {
            steps {
                script {

                    lock(resource: 'global-build-number-lock') {

                        // Use a fixed shared directory
                        ws("/var/lib/jenkins/global-counter") {

                            def counterFile = "global-build-number.txt"
                            def buildNumber

                            if (fileExists(counterFile)) {
                                buildNumber = readFile(counterFile).trim().toInteger()
                            } else {
                                buildNumber = 1
                            }

                            writeFile file: counterFile,
                                      text: (buildNumber + 1).toString()

                            env.GLOBAL_BUILD_NUMBER = buildNumber.toString()
                            currentBuild.displayName = "#${env.GLOBAL_BUILD_NUMBER}"

                            echo "Global Build Number: ${env.GLOBAL_BUILD_NUMBER}"
                        }
                    }
                }
            }
        }

        stage('Build') {
            steps {
                echo "Building with Global Build Number: ${env.GLOBAL_BUILD_NUMBER}"
            }
        }
    }
}
