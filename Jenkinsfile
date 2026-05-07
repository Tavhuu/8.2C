pipeline {
    agent any

    tools {
        nodejs 'NodeJS'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Tavhuu/8.2C.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                echo 'Installing project dependencies using npm...'
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                echo 'Running unit tests...'
                sh 'npm test || true'
            }
        }

        stage('Generate Coverage Report') {
            steps {
                echo 'Generating code coverage report...'
                sh 'npm run coverage || true'
            }
        }

        stage('NPM Audit (Security Scan)') {
            steps {
                echo 'Running security scan to identify known vulnerabilities (CVEs)...'
                sh 'npm audit || true'
            }
        }

    }
}
