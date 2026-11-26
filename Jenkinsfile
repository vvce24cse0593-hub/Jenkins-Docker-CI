pipeline {
    agent any

    environment {
        REGISTRY = "localhost:5000"
        IMAGE_NAME = "abh-app"
    }

    stages {
        stage('1. Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('2. Application Build & Test') {
            steps {
                bat '''
                    echo Checking Python installation...
                    where python
                    python --version

                    echo Running application...
                    python app.py
                '''
            }
        }

        stage('3. Build & Tag Docker Image') {
            steps {
                bat '''
                    docker build -t %REGISTRY%/%IMAGE_NAME%:%BUILD_NUMBER% -t %REGISTRY%/%IMAGE_NAME%:latest .
                '''
            }
        }

        stage('4. Push Image to Registry') {
            steps {
                bat '''
                    docker push %REGISTRY%/%IMAGE_NAME%:%BUILD_NUMBER%
                    docker push %REGISTRY%/%IMAGE_NAME%:latest
                '''
            }
        }
    }

    post {
        success {
            echo "Pipeline succeeded! Docker image pushed successfully."
        }

        failure {
            echo "Pipeline failed. Check Console Output for errors."
        }
    }
}
