pipeline {
    agent any

    stages {

        stage('Generate Global Build Number') {
            steps {
                script {
                    def counterBuild = build job: 'global-build-counter',
                                              wait: true,
                                              propagate: true

                    env.GLOBAL_BUILD_NUMBER = counterBuild.displayName.replace('#','')
                    env.RELEASE_VERSION = "1.0.${env.GLOBAL_BUILD_NUMBER}"

                    // Branch-aware Docker image tag
                    def branchTag = env.BRANCH_NAME.replaceAll('/', '-')
                    env.IMAGE_TAG = "${branchTag}-${env.GLOBAL_BUILD_NUMBER}"

                    currentBuild.displayName = "#${env.GLOBAL_BUILD_NUMBER}"
                    currentBuild.description = "Branch: ${env.BRANCH_NAME}"

                    echo "Global Build Number: ${env.GLOBAL_BUILD_NUMBER}"
                    echo "Release Version: ${env.RELEASE_VERSION}"
                    echo "Docker Image Tag: ${env.IMAGE_TAG}"
                }
            }
        }

        stage('Checkout') {
            steps {
                echo "Checking out source code..."
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo "Building application..."
                sh 'echo "Would run: mvn clean compile"'
            }
        }

        stage('Unit Tests') {
            steps {
                echo "Running unit tests..."
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
                echo "Running static code analysis..."
                sh 'echo "Would run: mvn sonar:sonar"'
            }
        }

        stage('Package') {
            steps {
                echo "Packaging artifact..."
                sh "echo 'Would run: mvn package -Drevision=${env.RELEASE_VERSION}'"
            }
        }

        stage('Build Docker Image') {
            when {
                branch 'main'
            }
            steps {
                echo "Building Docker image..."
                sh "echo 'Would run: docker build -t myapp:${env.IMAGE_TAG} .'"
            }
        }

        stage('Deploy') {
            when {
                branch 'main'
            }
            steps {
                echo "Deploying version ${env.RELEASE_VERSION}..."
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
