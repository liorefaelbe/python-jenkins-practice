pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo '========================================'
                echo 'Checking out source code from Git...'
                echo '========================================'
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                echo '========================================'
                echo 'Creating virtual environment...'
                echo 'Installing Python dependencies...'
                echo '========================================'

                sh 'python3 -m venv venv'
                sh '. venv/bin/activate && pip install -r requirements.txt'
            }
        }

        stage('Lint') {
            steps {
                echo '========================================'
                echo 'Running flake8 lint checks...'
                echo '========================================'

                sh '. venv/bin/activate && flake8 app tests --show-source --statistics'
            }
        }

        stage('Unit Tests') {
            steps {
                echo '========================================'
                echo 'Running Python unit tests with pytest...'
                echo '========================================'

                sh '. venv/bin/activate && PYTHONPATH=. pytest -v --junitxml=test-results.xml'
            }

            post {
                always {
                    junit 'test-results.xml'
                }
            }
        }
    }

    post {
        success {
            echo '========================================'
            echo 'PIPELINE PASSED SUCCESSFULLY'
            echo '========================================'
        }

        failure {
            echo '========================================'
            echo 'PIPELINE FAILED'
            echo 'Check the failed stage above.'
            echo '========================================'
        }
    }
}