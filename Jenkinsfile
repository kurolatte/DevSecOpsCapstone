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
                dependencyCheck additionalArguments: '--scan . --format XML --out reports',
                                odcInstallation: 'Default'
            }
            post {
                always {
                    dependencyCheckPublisher pattern: 'pattern: '**/dependency-check-report.xml'
'
                }
            }
        }


        stage('DAST - OWASP ZAP') {
            steps {
                echo 'DAST stage - this is where ZAP scan will go later.'
            }
        }
    }
}
