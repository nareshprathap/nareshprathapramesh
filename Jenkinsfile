pipeline {
    agent any

    stages {

        stage('Generate Global Build Number') {
            steps {
                script {

                    lock(resource: 'global-build-number-lock') {

                        // Get real Jenkins home directory (controller)
                        def jenkinsHome = jenkins.model.Jenkins.instance.rootDir.getAbsolutePath()
                        def counterFile = new File(jenkinsHome, "global-build-number.txt")

                        if (!counterFile.exists()) {
                            counterFile.write("1")
                        }

                        def buildNumber = counterFile.text.trim().toInteger()

                        // Increment and persist
                        counterFile.write((buildNumber + 1).toString())

                        env.GLOBAL_BUILD_NUMBER = buildNumber.toString()

                        currentBuild.displayName = "#${env.GLOBAL_BUILD_NUMBER}"

                        echo "Global Build Number: ${env.GLOBAL_BUILD_NUMBER}"
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
