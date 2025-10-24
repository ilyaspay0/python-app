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
                    echo "=== Creating virtual environment ==="
                    python3 -m venv venv
                    . venv/bin/activate
                    python3 -m pip install --upgrade pip
                    pip install -r requirements.txt
                '''
            }
        }

        stage('Run Tests') {
            steps {
                sh '''
                    echo "=== Running tests ==="
                    . venv/bin/activate
                    pytest || echo "Tests failed but continuing for demo"
                '''
            }
        }

        stage('SonarQube Analysis') {
            steps {
                echo "=== SonarQube Analysis (placeholder) ==="
            }
        }

        stage('Quality Gate') {
            steps {
                echo "=== Quality Gate Check (placeholder) ==="
            }
        }
    }
}
