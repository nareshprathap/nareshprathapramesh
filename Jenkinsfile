pipeline {
    agent any

    stages {
        stage('Get Global Build Number') {
            steps {
                script {
                    def counterBuild = build job: 'global-build-counter',
                                             wait: true
                    def globalNumber = counterBuild.getDescription()

                    // Fallback if needed
                    globalNumber = counterBuild.number

                    currentBuild.displayName = "#${globalNumber}"
                    env.GLOBAL_BUILD_NUMBER = "${globalNumber}"
                }
            }
        }

        stage('Build') {
            steps {
                echo "Global Build Number: ${env.GLOBAL_BUILD_NUMBER}"
            }
        }
    }
}
