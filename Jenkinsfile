pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Set up Python venv') {
            steps {
                bat """
                python -m venv venv
                call venv\\Scripts\\activate
                pip install --upgrade pip
                pip install -r requirements.txt
                """
            }
        }

        stage('Lint') {
            steps {
                bat """
                call venv\\Scripts\\activate
                pylint src || echo "pylint finished with warnings"
                """
            }
        }

        stage('Unit Tests with Coverage') {
            steps {
                bat """
                call venv\\Scripts\\activate
                pytest --cov=src --cov-report=xml:coverage.xml
                """
            }
        }
    }

    post {
        success {
            echo " CI basic pipeline finished successfully"
        }
        failure {
            echo "CI basic pipeline failed"
        }
    }
}
