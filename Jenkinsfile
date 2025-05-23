pipeline {
    agent any

    tools {
        nodejs "NodeJS"
    }

    environment {
        SONAR_TOKEN = credentials('SONAR_TOKEN') // ID of your SonarCloud token in Jenkins
    }

    stages {
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'npm test || exit 0' // Continue pipeline even if tests fail
            }
        }

        stage('NPM Audit (Security Scan)') {
            steps {
                sh 'npm audit || exit 0' // Continue pipeline even if audit reports issues
            }
        }

        stage('SonarCloud Analysis') {
            steps {
                withSonarQubeEnv('SonarCloud') {
                    sh '''
                        npx sonar-scanner \
                          -Dsonar.projectKey=sreeja2507_8.2CDevSecOps \
                          -Dsonar.organization=sreeja2507 \
                          -Dsonar.host.url=https://sonarcloud.io \
                          -Dsonar.login=$SONAR_TOKEN
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

   
                   
