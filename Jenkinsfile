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

        stage('SonarCloud Analysis') {
            steps {
                echo 'Downloading and running SonarScanner CLI for code quality analysis...'
                withCredentials([string(credentialsId: 'SONAR_TOKEN', variable: 'SONAR_TOKEN')]) {
                    sh """
                        curl -sSLo sonar-scanner.zip https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-5.0.1.3006-macosx.zip
                        unzip -o sonar-scanner.zip
                        chmod +x ./sonar-scanner-5.0.1.3006-macosx/bin/sonar-scanner
                        ./sonar-scanner-5.0.1.3006-macosx/bin/sonar-scanner \
                            -Dsonar.projectKey=Tavhuu_8.2C \
                            -Dsonar.organization=tavhuu \
                            -Dsonar.host.url=https://sonarcloud.io \
                            -Dsonar.login=\$SONAR_TOKEN \
                            -Dsonar.sources=. \
                            -Dsonar.exclusions=node_modules/**,test/** \
                            -Dsonar.javascript.lcov.reportPaths=coverage/lcov.info
                    """
                }
            }
        }

    }
}
