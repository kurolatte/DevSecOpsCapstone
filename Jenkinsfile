pipeline {
    agent any

    stages {

        stage('Checkout Code') {
            steps {
                echo 'Code already checked out by Jenkins (SCM stage).'
            }
        }

        stage('Build') {
            steps {
                echo 'Build stage - placeholder for now.'
            }
        }

        stage('SAST - SonarQube') {
            steps {
                echo 'SAST stage - this is where SonarQube scan will go later.'
            }
        }

                stage('SCA - Dependency-Check') {
    steps {
        echo 'SCA stage: OWASP Dependency-Check is used to scan project dependencies for known vulnerabilities. (In this environment, we run it manually via CLI and use the report in our analysis.)'
    }
}

        stage('DAST - OWASP ZAP') {
            steps {
                echo 'DAST stage - this is where ZAP scan will go later.'
            }
        }
    }
}
