pipeline {
    agent any

    stages {
        stage('Get Global Build Number') {
            steps {
                script {
                    // Run the shell script with flock to safely increment
                    env.GLOBAL_BUILD_NUMBER = sh(
                        script: '''
                            COUNTER_FILE="$JENKINS_HOME/global_build_number.txt"

                            # Ensure directory exists
                            mkdir -p "$(dirname "$COUNTER_FILE")"

                            # Use file lock to avoid parallel race conditions
                            (
                                flock -n 200 || { echo "Another build is running. Waiting..."; flock 200; }

                                if [ ! -f "$COUNTER_FILE" ]; then
                                    echo 1 > "$COUNTER_FILE"
                                fi

                                BUILD_NUM=$(cat "$COUNTER_FILE")
                                NEXT_BUILD=$((BUILD_NUM + 1))
                                echo $NEXT_BUILD > "$COUNTER_FILE"

                                echo $NEXT_BUILD
                            ) 200> "$COUNTER_FILE.lock"
                        ''',
                        returnStdout: true
                    ).trim()

                    // Display the global build number in Jenkins UI
                    currentBuild.displayName = "#${env.GLOBAL_BUILD_NUMBER}"
                    echo "Assigned Global Build Number: ${env.GLOBAL_BUILD_NUMBER}"
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
