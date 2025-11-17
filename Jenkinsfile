pipeline {
    agent any
    stages {
        stage('Checkout Code') {
            steps {
                git 'https://github.com/kurolatte/DevSecOpsCapstone.git'
            }
        }
        stage('Build') {
            steps {
                echo 'Build successful — Jenkins is working!'
            }
        }
    }
}
