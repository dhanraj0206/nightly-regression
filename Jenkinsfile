pipeline {

    agent any

    tools {
        jdk 'JDK21'
        maven 'Maven'
    }

    triggers {
        cron('0 0 * * *')
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/USERNAME/nightly-regression.git'
            }
        }

        stage('Build') {
            steps {
                bat 'mvn clean compile'
            }
        }

        stage('Regression Testing') {
            steps {
                bat 'mvn test'
            }
        }

        stage('Archive Reports') {
            steps {
                archiveArtifacts artifacts:
                    'target/surefire-reports/*.xml',
                    fingerprint: true
            }
        }
    }

    post {
        always {
            junit allowEmptyResults: true,
                  testResults: 'target/surefire-reports/*.xml'
        }

        success {
            echo 'Nightly Regression Tests Passed!'
        }

        failure {
            echo 'Nightly Regression Tests Failed!'
        }
    }
}