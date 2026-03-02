pipeline {
    agent any

    stages {

        stage('Generate Global Build Number') {
            steps {
                script {
                    def counterBuild = build job: 'global-build-counter',
                                              wait: true,
                                              propagate: true

                    env.GLOBAL_BUILD_NUMBER =
                        counterBuild.displayName.replace('#','')

                    env.IMAGE_TAG = env.GLOBAL_BUILD_NUMBER
                    env.RELEASE_VERSION = "1.0.${env.GLOBAL_BUILD_NUMBER}"

                    currentBuild.displayName =
                        "#${env.GLOBAL_BUILD_NUMBER}"

                    echo "Global Build Number: ${env.GLOBAL_BUILD_NUMBER}"
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
                sh 'mvn clean compile'
            }
        }

        stage('Unit Tests') {
            steps {
                echo "Running unit tests..."
                sh 'mvn test'
            }
            post {
                always {
                    junit '**/target/surefire-reports/*.xml'
                }
            }
        }

        stage('Static Analysis') {
            steps {
                echo "Running static code analysis..."
                sh 'mvn sonar:sonar || true'
            }
        }

        stage('Package') {
            steps {
                echo "Packaging artifact..."
                sh "mvn package -Drevision=${env.RELEASE_VERSION}"
            }
        }

        stage('Build Docker Image') {
            when {
                branch 'main'
            }
            steps {
                echo "Building Docker image..."
                sh """
                   docker build -t myapp:${env.IMAGE_TAG} .
                """
            }
        }

        stage('Deploy') {
            when {
                branch 'main'
            }
            steps {
                echo "Deploying version ${env.RELEASE_VERSION}..."
                sh "./deploy.sh ${env.RELEASE_VERSION}"
            }
        }
    }

    post {
        success {
            echo "Build ${env.GLOBAL_BUILD_NUMBER} completed successfully!"
        }
        failure {
            echo "Build ${env.GLOBAL_BUILD_NUMBER} failed!"
        }
        always {
            cleanWs()
        }
    }
}
