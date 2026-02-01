pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out source code from GitHub...'
                checkout scm
            }
        }

        stage('Setup Python Environment') {
            steps {
                echo 'Setting up Python virtual environment...'
                sh '''
                python3 -m venv venv
                . venv/bin/activate
                pip install --upgrade pip
                pip install -r requirements.txt
                pip install pytest pytest-html pytest-cov
                '''
            }
        }

        stage('Build / Syntax Check') {
            steps {
                echo 'Checking Python syntax...'
                sh '''
                . venv/bin/activate
                python3 -m compileall .
                '''
            }
        }

        stage('Unit Test') {
            steps {
                echo 'Running unit tests with pytest...'
                sh '''
                . venv/bin/activate
                mkdir -p test-reports
                pytest --junitxml=test-reports/results.xml
                '''
            }
            post {
                always {
                    echo 'Archiving test results...'
                    junit 'test-reports/results.xml'
                }
            }
        }
    }

    post {
        success {
            echo 'Python CI Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Please check the test results.'
        }
    }
}
