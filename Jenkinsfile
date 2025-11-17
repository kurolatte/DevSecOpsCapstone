pipeline {
    agent any
    stages {

        stage('Checkout Code') {
            git branch: 'main', url: 'https://github.com/kurolatte/DevSecOpsCapstone.git' }
        }

        stage('Build') {
            steps { echo 'App build successful!' }
        }

        stage('SAST - SonarQube') {
            steps {
                withSonarQubeEnv('Sonar') {
                    sh 'sonar-scanner'
                }
            }
        }

        stage('SCA - Dependency-Check') {
            steps {
                dependencyCheck additionalArguments: '--scan .'
            }
        }

        stage('Start Juice Shop') {
            steps {
                sh 'docker run -d -p 3000:3000 --name juice owasp/juice-shop'
            }
        }

        stage('DAST - ZAP') {
            steps {
                sh '''
                docker run --rm owasp/zap2docker-stable zap-baseline.py \
                -t http://localhost:3000 -r zap-report.html
                '''
            }
        }
    }
}
