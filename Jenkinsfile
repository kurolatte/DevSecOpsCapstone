pipeline {
    agent any

    stages {

        stage('Checkout Code') {
            steps {
                // Jenkins already does Declarative: Checkout SCM before this,
                // but we keep this here for clarity/future freestyle jobs.
                echo 'Code already checked out by Jenkins (SCM stage).'
            }
        }

        stage('Build') {
            steps {
                echo 'Build stage - placeholder for now.'
                // e.g. npm install / mvn package / etc. if needed
            }
        }

        stage('SAST - SonarQube') {
            steps {
                echo 'SAST stage - SonarQube analysis is run externally via sonar-scanner CLI.'
                // For the capstone you already ran sonar-scanner from your PC.
                // If you later want to run it here, you can add a sh "sonar-scanner ..." command.
            }
        }

        stage('SCA - OWASP Dependency-Check') {
            steps {
                echo 'Running OWASP Dependency-Check using Docker...'
                sh '''
                    mkdir -p odc-reports
                    docker run --rm \
                      -v "$PWD:/src" \
                      -v "$HOME/depcheck-data:/usr/share/dependency-check/data" \
                      -v "$PWD/odc-reports:/report" \
                      owasp/dependency-check:latest \
                      --scan /src/juice-shop-master \
                      --format "HTML,XML" \
                      --out /report \
                      --project "DevSecOpsCapstone"
                '''
                echo 'Archiving OWASP Dependency-Check reports...'
                archiveArtifacts artifacts: 'odc-reports/*', fingerprint: true, onlyIfSuccessful: false
            }
        }

        stage('SCA - Retire.js (initial testing)') {
            steps {
                echo 'Running Retire.js SCA scan (JavaScript dependencies)...'
                // Assumes retire is installed inside the Jenkins container: npm install -g retire
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
