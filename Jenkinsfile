pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps { checkout scm }
        }

        stage('Setup Python') {
            steps {
                bat """
                python -m venv venv
                call venv\\Scripts\\activate
                pip install --upgrade pip
                pip install -r requirements.txt
                """
            }
        }

        stage('Run Lint') {
            steps {
                bat """
                call venv\\Scripts\\activate
                pylint src || echo "Lint completed with warnings"
                """
            }
        }

        stage('Run Unit Tests + Coverage') {
            steps {
                bat """
                call venv\\Scripts\\activate
                pytest --cov=src --cov-report=xml:coverage.xml
                """
            }
        }
    }

    post {
        success { echo "✓ Build Successful" }
        failure { echo "✗ Build Failed" }
    }
}
