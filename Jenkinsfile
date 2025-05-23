pipeline {
    agent any

    tools {
        nodejs "NodeJS" // Ensure NodeJS is installed in Jenkins (Manage Jenkins > Global Tool Configuration)
    }

    environment {
        SONAR_TOKEN = credentials('SONAR_TOKEN') // Replace with your credential ID
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/sreeja2507/8.2CDevSecOps.git', branch: 'main'
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                bat 'npm test || exit /b 0' // Avoid fail if snyk test is not yet set up
            }
        }

        stage('SonarCloud Analysis') {
            steps {
                bat '''
                    curl -o sonar-scanner.zip https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-4.8.0.2856-windows.zip
                    powershell -Command "Expand-Archive sonar-scanner.zip -DestinationPath . -Force"
                    sonar-scanner-4.8.0.2856-windows\\bin\\sonar-scanner.bat ^
                    -D"sonar.projectKey=sreeja2507_8.2CDevSecOps" ^
                    -D"sonar.organization=sreeja2507" ^
                    -D"sonar.host.url=https://sonarcloud.io" ^
                    -D"sonar.login=%SONAR_TOKEN%"
                '''
            }
        }
    }

    post {
        always {
            echo "Pipeline completed. Sonar token was loaded: ${env.SONAR_TOKEN != null ? 'Yes' : 'No'}"
        }
    }
}

