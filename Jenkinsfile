pipeline {
    agent any

    stages {
        stage('Generate Version') {
            steps {
                script {
                    def version = VersionNumber(
                        versionNumberString: '${BUILDS_ALL_TIME}'
                    )

                    currentBuild.displayName = "#${version}"
                    env.GLOBAL_BUILD_NUMBER = version
                }
            }
        }

        stage('Build') {
            steps {
                echo "Build Version: ${env.GLOBAL_BUILD_NUMBER}"
            }
        }
    }
}
