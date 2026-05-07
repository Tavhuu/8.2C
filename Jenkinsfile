pipeline {
    agent any

    tools {
        nodejs 'NodeJS'
    }

    environment {
        SONAR_TOKEN = credentials('7023c6e4d71b7494fe10be14212a3fbd34c92fbc')
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
        sh '''
            # Download Mac (macOS) version of SonarScanner
            curl -sSLo sonar-scanner.zip https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-5.0.1.3006-macosx.zip

            # Unzip it
            unzip -o sonar-scanner.zip

            # Make it executable
            chmod +x ./sonar-scanner-5.0.1.3006-macosx/bin/sonar-scanner

            # Run SonarScanner
            ./sonar-scanner-5.0.1.3006-macosx/bin/sonar-scanner \
                -Dsonar.projectKey=Tavhuu_8.2C \
                -Dsonar.organization=tavhuu \
                -Dsonar.host.url=https://sonarcloud.io \
                -Dsonar.login=${SONAR_TOKEN} \
                -Dsonar.sources=. \
                -Dsonar.exclusions=node_modules/**,test/** \
                -Dsonar.javascript.lcov.reportPaths=coverage/lcov.info
        '''
    }
}
