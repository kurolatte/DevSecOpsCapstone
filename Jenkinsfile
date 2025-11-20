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
                echo 'SAST - SonarQube analysis is executed via sonar-scanner (outside this Jenkinsfile for now).'
            }
        }

        stage('SCA - OWASP Dependency-Check') {
    steps {
        echo "Running OWASP Dependency-Check using Jenkins plugin..."

        dependencyCheck additionalArguments: '--format HTML', 
                        odcInstallation: 'DC', 
                        out: 'odc-reports', 
                        scanpath: 'juice-shop-master'
    }
}


        stage('DAST - OWASP ZAP') {
            steps {
                echo 'DAST - OWASP ZAP baseline scan is executed via Docker outside this Jenkinsfile.'
            }
        }
    }

    post {
        always {
            echo 'Publishing Dependency-Check results and archiving reports...'
            dependencyCheckPublisher pattern: 'odc-reports/dependency-check-report.xml'
            archiveArtifacts artifacts: 'odc-reports/*', fingerprint: true, onlyIfSuccessful: false
        }
    }
}
