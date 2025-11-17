pipeline {
    agent any

    environment {
        APP_ENV = 'development'
        VERSION = '1.0.0'
    }

    stages {
        stage('Build') {
            steps {
                echo "Building version ${VERSION} in ${APP_ENV} environment"
            }
        }

        stage('Show Env Vars') {
            steps {
                // Windows:
                bat 'set'

                // If you were on Linux/macOS instead, you'd use:
                // sh 'printenv'
            }
        }

        stage('Test') {
            when {
                expression { return params.EXECUTE_TESTS }
            }
            steps {
                echo "Running tests for version ${VERSION}"
            }
        }

        stage('Deploy') {
            steps {
                echo "Deploying version ${VERSION}"
            }
        }
    }

    post {
        success {
            echo "Pipeline for version ${VERSION} completed successfully!"
        }
        failure {
            echo "Pipeline for version ${VERSION} failed. Please check the logs."
        }
        always {
            echo 'This always runs at the end, success or failure.'
        }
    }
}
