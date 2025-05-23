pipeline {
    agent any

    tools {
        nodejs 'NodeJS' 
    }

    environment {
        SONAR_SCANNER_HOME = 'C:\\Users\\HP\\Downloads\\sonar-scanner-cli-7.1.0.4889-windows-x64\\sonar-scanner-7.1.0.4889-windows-x64'
        PATH = "${SONAR_SCANNER_HOME}\\bin;${env.PATH}"
    }

    stages {
        stage('Tool Install') {
            steps {
                echo 'Installing NodeJS...'
            }
        }

        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                bat 'npm test || exit /b 0'
            }
        }

        stage('Security Scan - npm audit') {
            steps {
                bat 'npm audit || exit /b 0'
            }
        }

        stage('SonarCloud Analysis') {
            steps {
                withCredentials([string(credentialsId: 'sonarcloud-token', variable: 'SONAR_TOKEN')]) {
                    bat """
                        sonar-scanner ^
                        -D"sonar.projectKey=sreeja2507_8.2CDevSecOps" ^
                        -D"sonar.organization=sreeja2507" ^
                        -D"sonar.sources=." ^
                        -D"sonar.host.url=https://sonarcloud.io" ^
                        -D"sonar.login=%SONAR_TOKEN%"
                    """
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline completed.'
        }
    }
}
