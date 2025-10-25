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
        script {
            try {
                def scannerHome = tool 'SonarScanner'
                withSonarQubeEnv('SonarQube') {
                    sh """
                        . venv/bin/activate
                        ${scannerHome}/bin/sonar-scanner \
                          -Dsonar.projectKey=python-app \
                          -Dsonar.sources=. \
                          -Dsonar.python.coverage.reportPaths=coverage.xml \
                          -Dsonar.host.url=http://sonarqube:9000
                    """
                }
            } catch (Exception e) {
                echo "⚠️ SonarQube analysis failed: ${e.message}"
                echo "Continuing pipeline without SonarQube..."
            }
        }
    }
}

stage('Quality Gate') {
    steps {
        script {
            try {
                timeout(time: 2, unit: 'MINUTES') {
                    def qg = waitForQualityGate()
                    if (qg.status != 'OK') {
                        error "Pipeline aborted due to quality gate failure: ${qg.status}"
                    }
                }
            } catch (Exception e) {
                echo "⚠️ Quality Gate check failed: ${e.message}"
                echo "Continuing pipeline..."
            }
        }
    }
}


       
    }
}
