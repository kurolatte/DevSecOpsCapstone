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
                echo 'SAST stage - SonarQube analysis is run externally via sonar-scanner CLI.'
        }

        stage('SCA - OWASP Dependency-Check') {
    steps {
        echo 'Running OWASP Dependency-Check via Jenkins plugin...'

        sh 'mkdir -p odc-reports'

        dependencyCheck additionalArguments: '--scan . --format XML --out odc-reports', odcInstallation: 'DC'
    }
    post {
        always {
            echo 'Publishing Dependency-Check results...'
            dependencyCheckPublisher pattern: 'odc-reports/dependency-check-report.xml'
            archiveArtifacts artifacts: 'odc-reports/*', fingerprint: true, onlyIfSuccessful: false
        }


        stage('SCA - Retire.js (initial testing)') {
            steps {
                echo 'Running Retire.js SCA scan (JavaScript dependencies)...'
                sh '''
                    retire --path . --outputformat json --outputpath retire-report.json || true
                '''
                echo 'Archiving Retire.js report...'
                archiveArtifacts artifacts: 'retire-report.json', fingerprint: true, onlyIfSuccessful: false
            }
        }

        stage('DAST - OWASP ZAP') {
            steps {
                echo 'Running OWASP ZAP Baseline Scan via Docker...'
                sh '''
                    docker run --rm \
                      -v "$PWD:/zap/wrk" \
                      -t zaproxy/zap-stable zap-baseline.py \
                      -t http://host.docker.internal:3000 \
                      -r zap-report.html || true
                '''
                echo 'Archiving ZAP DAST report...'
                archiveArtifacts artifacts: 'zap-report.html', fingerprint: true, onlyIfSuccessful: false
            }
        }
    }
}
