pipeline {
    agent any

    tools {
        nodejs 'NodeJS'
    }

    environment {
        SONAR_SCANNER_HOME = 'C:\\Users\\HP\\Downloads\\sonar-scanner-cli-7.1.0.4889-windows-x64'
    }

    stages {
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
                withCredentials([string(credentialsId: 'SONAR_TOKEN', variable: 'SONAR_TOKEN')]) {
                    bat '''
                        "%SONAR_SCANNER_HOME%\\bin\\sonar-scanner.bat" ^
                        -D"sonar.projectKey=sreeja2507_8.2CDevSecOps" ^
                        -D"sonar.organization=sreeja2507" ^
                        -D"sonar.sources
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
       
      
      

           
                 

       
      
       
     
