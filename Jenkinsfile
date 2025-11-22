pipeline {
    agent any

    stages {

        stage('Checkout Code') {
            steps {
                echo 'Code already checked out by Jenkins (SCM stage).'
            }
        }

        stage('Build / Install Dependencies') {
    steps {
        echo 'Installing Juice Shop dependencies so SCA tools can scan properly...'

        dir('juice-shop-master') {
            sh '''
                if [ -f package-lock.json ]; then
                    npm ci --force
                else
                    npm install --force
                fi
            '''
        }
    }
}


        stage('SAST - SonarQube') {
            steps {
                echo 'SAST - SonarQube analysis is executed via sonar-scanner (outside this Jenkinsfile for now).'
            }
        }

        stage('SCA - OWASP Dependency-Check') {
            steps {
                echo 'Running OWASP Dependency-Check using Jenkins plugin...'
                dependencyCheck(
                odcInstallation: 'DC',
                additionalArguments: '--scan .'
             )
        }
    }

        stage('SCA - Retire.js (JavaScript dependencies)') {
            steps {
                echo 'Running Retire.js SCA scan for JavaScript dependencies...'
                sh '''
                    retire --path . --outputformat json --outputpath retire-report.json || true
                '''

                echo 'Archiving Retire.js report...'
                archiveArtifacts artifacts: 'retire-report.json', fingerprint: true, onlyIfSuccessful: false
            }
        }

        stage('DAST - OWASP ZAP') {
            steps {
                echo 'DAST - OWASP ZAP baseline scan is executed via Docker on the host machine.'
                echo 'For this project, ZAP is run with:'
                echo '  docker run -v "<project>:/zap/wrk" -t zaproxy/zap-stable zap-baseline.py -t http://host.docker.internal:3000 -r zap-report.html'
            }
        }
    }

    post {
        always {
            echo 'Publishing Dependency-Check results and archiving reports...'
            dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
            archiveArtifacts artifacts: 'dependency-check-report.*', fingerprint: true, onlyIfSuccessful: false

        }
    }
}
