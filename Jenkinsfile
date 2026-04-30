pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out source code...'
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                echo 'Installing Python dependencies...'
                sh 'python3 -m venv venv'
                sh '. venv/bin/activate && pip install -r requirements.txt'
            }
        }

        stage('Lint') {
            steps {
                echo 'Running flake8...'
                sh '. venv/bin/activate && flake8 app tests'
            }
        }

        stage('Unit Tests') {
            steps {
                echo 'Running unit tests...'
                sh '. venv/bin/activate && PYTHONPATH=. pytest -v'
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully.'
        }

        failure {
            echo 'Pipeline failed. Check the logs.'
        }
    }
}