pipeline {
    agent any

    environment {
        SONAR_PROJECT_KEY = 'Koosha2000_8.2CDevSecOps'
        SONAR_ORGANIZATION = 'Koosha2000'
        SONAR_TOKEN = credentials('sonar-token')
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/Koosha2000/8.2CDevSecOps.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'npm test || true'
            }
        }

        stage('Generate Coverage Report') {
            steps {
                sh 'npm run coverage || true'
            }
        }

        stage('NPM Audit (Security Scan)') {
            steps {
                sh 'npm audit || true'
            }
        }

        stage('SonarCloud Analysis') {
            steps {
                withCredentials([string(credentialsId: 'sonar-token', variable: 'SONAR_TOKEN')]) {
                    sh '''
                        curl -sSLo sonar-scanner.zip https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-5.0.1.3006-linux.zip
                        unzip -o sonar-scanner.zip
                        cd sonar-scanner-5.0.1.3006-linux
                        export PATH=$PWD/bin:$PATH
                        ./bin/sonar-scanner
                    '''
                }
            }
        }
    }
}
