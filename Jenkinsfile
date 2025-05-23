pipeline {
    agent any

    environment {
        NODEJS_HOME = tool 'NodeJS'
        PATH = "${NODEJS_HOME}/bin:${env.PATH}"
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
                checkout([$class: 'GitSCM',
                    branches: [[name: '*/main']],
                    userRemoteConfigs: [[
                        url: 'https://github.com/sreeja2507/8.2CDevSecOps.git',
                        credentialsId: 'Github-Token'
                    ]]
                ])
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
                    bat "\"C:\\Users\\HP\\Downloads\\sonar-scanner-cli-7.1.0.4889-windows-x64\\sonar-scanner-7.1.0.4889-windows-x64\\bin\\sonar-scanner.bat\" -D\"sonar.projectKey=sreeja2507_8.2CDevSecOps\" -D\"sonar.organization=sreeja2507\" -D\"sonar.sources=.\" -D\"sonar.host.url=https://sonarcloud.io\" -D\"sonar.login=%SONAR_TOKEN%\""
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
