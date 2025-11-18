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

                stage('SCA - Retire.js') {
    steps {
        sh '''
        echo "Running Retire.js SCA scan..."
        retire --path . --outputformat json --outputpath retire-report.json || true
        '''
    }
    post {
        always {
            echo "Archiving Retire.js report..."
            archiveArtifacts artifacts: 'retire-report.json', onlyIfSuccessful: false
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
