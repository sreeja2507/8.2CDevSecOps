pipeline {
    agent any

    environment {
        NODEJS_HOME = "${tool 'NodeJS'}"
        PATH = "${env.NODEJS_HOME}/bin:${env.PATH}"
    }

    stages {
        stage('Tool Install') {
            steps {
                echo 'Installing NodeJS...'
                tool name: 'NodeJS', type: 'jenkins.plugins.nodejs.tools.NodeJSInstallation'
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
                withEnv(["PATH+SONAR=${tool 'SonarScanner'}\\bin"]) {
                    withCredentials([string(credentialsId: 'sonarcloud-token', variable: 'SONAR_TOKEN')]) {
                        bat '''
                            sonar-scanner ^
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
    }

    post {
        always {
            echo 'Pipeline completed.'
        }
    }
}

      
       
     
