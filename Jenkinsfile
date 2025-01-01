pipeline {
    agent any
    tools {
        nodejs 'nodejs' 
    }
 
    environment {
        NODEJS_HOME = 'C:/Program Files/nodejs'  
        SONAR_SCANNER_PATH = 'C:/Program Files/sonar-scanner-6.2.1.4610-windows-x64/bin'
    }
 
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
 
       stage('Install Dependencies') {
            steps {
                // Set the PATH and install dependencies using npm
                bat '''
                set PATH=%NODEJS_HOME%;%PATH%
                npm install
                '''
            }
        }


 
        stage('SonarQube Analysis') {
            environment {
                SONAR_TOKEN = credentials('Sonarqube-token') // Accessing the SonarQube token stored in Jenkins credentials
            }
            steps {
                // Ensure that sonar-scanner is in the PATH
                bat '''
                set PATH=%SONAR_SCANNER_PATH%;%PATH%
                where sonar-scanner || echo "SonarQube scanner not found. Please install it."
                sonar-scanner -Dsonar.projectKey=backend-task1^
                    -Dsonar.sources=. ^
                    -Dsonar.host.url=http://localhost:9000 ^
                    -Dsonar.token=sqp_62b19649a9e3e5f15df9fc4c89708622b04643e4
                '''
            }
        }
    }
 
    post {
        success {
            echo 'Pipeline completed successfully'
        }
        failure {
            echo 'Pipeline failed'
        }
        always {
            echo 'This runs regardless of the result.'
        }
    }
}
