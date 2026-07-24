pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Python Setup') {
            steps {
                sh '''
                    python3 --version
                    python3 -m venv .jenkins-venv
                    . .jenkins-venv/bin/activate
                    python -m pip install --upgrade pip
                    pip install -r requirements.txt
                '''
            }
        }

        stage('Unit Tests') {
            steps {
                sh '''
                    . .jenkins-venv/bin/activate
                    pytest -v
                '''
            }
        }

       stage('Build Backend Image') {
        steps {
            sh '''
                docker build \
                -t fastapi-backend:${BUILD_NUMBER} \
                ./backend
            '''
        }
    }

    stage('Build Frontend Image') {
            steps {
                sh '''
                    docker build \
                    -t fastapi-frontend:${BUILD_NUMBER} \
                    ./frontend
                '''
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully.'
        }

        failure {
            echo 'Pipeline failed. Check the Jenkins console output.'
        }

        always {
            deleteDir()
        }
    }
}