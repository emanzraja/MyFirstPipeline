pipeline {
    agent any

    tools {
        maven 'Maven-3.9.1'
    }

    environment {
        APP_ENV = 'development'
        VERSION = '1.0.0'
    }

    parameters {
        booleanParam(
            name: 'EXECUTE_TESTS',
            defaultValue: true,
            description: 'Run the Test stage?'
        )
    }

    stages {
        stage('Build') {
            steps {
                echo "Building ${VERSION} in ${APP_ENV}"
                bat 'mvn -v'   // Windows-specific command to print Maven version
            }
        }

        stage('Test') {
            when {
                expression { return params.EXECUTE_TESTS }
            }
            steps {
                echo "Running tests because EXECUTE_TESTS = true"
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
            echo "Pipeline for ${VERSION} completed successfully!"
        }
        failure {
            echo "Pipeline for ${VERSION} failed."
        }
        always {
            echo "Pipeline completed (post block)."
        }
    }
}
