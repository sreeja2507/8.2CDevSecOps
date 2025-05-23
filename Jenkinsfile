pipeline {
    agent any

    tools {
        nodejs 'NodeJS' // This must match the name configured under Manage Jenkins > Global Tool Configuration
    }

    environment {
        SONAR_SCANNER_HOME = 'sonar-scanner-4.8.0.2856-windows' // or path where your scanner is extracted
    }

    stages {
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
                withCredentials([string(credentialsId: 'SONAR_TOKEN', variable: 'SONAR_TOKEN')]) {
                    bat '''
                        ${SONAR_SCANNER_HOME}\\bin\\sonar-scanner.bat ^
                        -D"sonar.projectKey=sreeja2507_8.2CDevSecOps" ^
                        -D"sonar.organization=sreeja2507" ^
                        -D"sonar.sources=." ^
                        -D"sonar.host.url=https://sonarcloud.io" ^
                        -D"sonar.login=%SONAR_TOKEN%"
                    '''
                }
            }
        }
    }

    post {
        always {
            echo "Pipeline completed. Sonar token was loaded: ${env.SONAR_TOKEN != null ? 'Yes' : 'No'}"
        }
    }
}


       
      
       
     
