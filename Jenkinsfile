pipeline {
    agent any

    environment {
        APP_ENV = 'development'
        VERSION = '1.0.0'
        VENV_DIR = 'venv'
        DEPLOY_DIR = 'C:\\flask_deploy'
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
                echo "Building Flask app ${VERSION} in ${APP_ENV}"
                bat '''
                python --version
                python -m venv %VENV_DIR%
                %VENV_DIR%\\Scripts\\pip install --upgrade pip
                %VENV_DIR%\\Scripts\\pip install -r requirements.txt
                '''
            }
        }

        stage('Test') {
            when {
                expression { params.EXECUTE_TESTS }
            }
            steps {
                echo "Running tests because EXECUTE_TESTS = true"
                bat '''
                %VENV_DIR%\\Scripts\\pytest
                '''
            }
        }

        stage('Deploy') {
            steps {
                echo "Deploying Flask app version ${VERSION}"
                bat '''
                if exist build rmdir /s /q build
                mkdir build
                xcopy app.py build\\ /Y
                xcopy requirements.txt build\\ /Y

                if not exist %DEPLOY_DIR% mkdir %DEPLOY_DIR%
                xcopy build\\* %DEPLOY_DIR%\\ /Y
                '''
            }
        }
    }

    post {
        success {
            echo "Pipeline for Flask ${VERSION} completed successfully!"
        }
        failure {
            echo "Pipeline for Flask ${VERSION} failed."
        }
        always {
            echo "Pipeline completed (post block)."
        }
    }
}
