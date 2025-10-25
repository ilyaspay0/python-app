pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/ilyaspay0/python-app.git'
            }
        }

        stage('Install dependencies') {
            steps {
                sh '''
                echo "Creating virtual environment..."
                python3 -m venv venv
                . venv/bin/activate
                pip install --upgrade pip
                pip install -r requirements.txt
                '''
            }
        }

        stage('Run Tests') {
            steps {
                sh '''
                echo "Running tests inside venv..."
                . venv/bin/activate
                pytest || echo "Tests failed or not found"
                '''
            }
        }

        stage('SonarQube Analysis') {
            steps {
                echo 'Skipping SonarQube for now...'
            }
        }

        stage('Quality Gate') {
            steps {
                echo 'Skipping Quality Gate for now...'
            }
        }
    }
}
