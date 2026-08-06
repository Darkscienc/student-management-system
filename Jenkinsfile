node {

    stage('Checkout') {
        checkout scm
    }

    stage('Check Python') {
        sh 'python3 --version'
    }

    stage('Create Virtual Environment') {
        sh 'python3 -m venv venv'
    }

    stage('Install Dependencies') {
        sh '''
            . venv/bin/activate
            pip install --upgrade pip
            pip install -r requirements.txt
        '''
    }

    stage('Run Application') {
        sh '''
            . venv/bin/activate
            python app.py
        '''
    }

    stage('Run Unit Tests') {
        sh '''
            . venv/bin/activate
            pytest --junitxml=test-results.xml
        '''
    }

    stage('Code Quality Check') {
        sh '''
            . venv/bin/activate
            flake8 .
        '''
    }

    stage('Generate Coverage Report') {
        sh '''
            . venv/bin/activate
            coverage run -m pytest
            coverage xml
        '''
    }

    stage('Archive Reports') {
        archiveArtifacts artifacts: '*.xml', fingerprint: true
        
        junit 'test-results.xml'
    }

    stage('Build Completed') {
        echo 'CI Pipeline Completed Successfully'
    }
}