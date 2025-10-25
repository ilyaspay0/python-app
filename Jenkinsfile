pipeline {
    agent any

    environment {
        SONARQUBE_SCANNER_HOME = tool 'SonarScanner'
    }

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
                    pip install pytest pytest-cov
                '''
            }
        }

        stage('Run Tests and Coverage') {
    steps {
        sh '''
            . venv/bin/activate
            echo "Running tests with coverage..."
            pytest --cov=. --cov-report xml --cov-report term --junitxml=report.xml
        '''
        // Archive JUnit test report
        junit 'report.xml'
        // Optional: publish coverage in Sonar later
    }
}


       stage('SonarQube Analysis') {
    steps {
        withSonarQubeEnv('SonarQube') {
            sh """
                . venv/bin/activate
                sonar-scanner \
                  -Dsonar.projectKey=python-app \
                  -Dsonar.sources=. \
                  -Dsonar.python.coverage.reportPaths=coverage.xml \
                  -Dsonar.host.url=http://localhost:9000
            """
        }
    }
}


        stage('Quality Gate') {
            steps {
                timeout(time: 2, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }
}
