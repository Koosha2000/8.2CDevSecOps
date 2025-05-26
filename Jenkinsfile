pipeline {
    agent any

    environment {
        // Optional: specify Node version if needed
        NODE_ENV = 'development'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'npm test'
            }
        }

        stage('SonarCloud Analysis') {
            environment {
                SONAR_SCANNER_HOME = './sonar-scanner'
            }

            steps {
                withCredentials([string(credentialsId: 'SONAR_TOKEN', variable: 'SONAR_TOKEN')]) {
                    sh '''
                        curl -sSLo sonar-scanner.zip https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-4.8.0.2856-linux.zip
                        unzip -o sonar-scanner.zip
                        chmod +x sonar-scanner-*/bin/sonar-scanner
                        ./sonar-scanner-*/bin/sonar-scanner \
                          -Dsonar.organization=koosha \
                          -Dsonar.projectKey=Koosha2000_8.2CDevSecOps \
                          -Dsonar.sources=. \
                          -Dsonar.host.url=https://sonarcloud.io \
                          -Dsonar.login=$SONAR_TOKEN
                    '''
                }
            }
        }
    }
}
