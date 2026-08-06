pipeline {

    agent {
        docker {
            image 'python:3.12'
        }
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'pip install -r requirements.txt'
            }
        }

        stage('Run Application') {
            steps {
                sh 'python app.py'
            }
        }

        stage('Run Unit Tests') {
            steps {
                sh 'pytest'
            }
        }

        stage('Code Quality') {
            steps {
                sh 'flake8 .'
            }
        }

        stage('Generate Coverage') {
            steps {
                sh 'coverage run -m pytest'
                sh 'coverage xml'
            }
        }

        stage('Archive Reports') {
            steps {
                archiveArtifacts artifacts: '*.xml', fingerprint: true
            }
        }

    }

    post {
        success {
            echo 'Pipeline Completed Successfully'
        }

        failure {
            echo 'Pipeline Failed'
        }
    }
}