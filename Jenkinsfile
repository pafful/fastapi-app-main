pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('List Files') {
            steps {
                sh '''
                    pwd
                    ls -la
                    find . -maxdepth 2 -type f | sort
                '''
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                    cd backend
                    python3 -m venv .venv
                    . .venv/bin/activate
                    python -m pip install --upgrade pip
                    pip install uv
                    uv sync --frozen
                '''
            }
        }

        stage('Run Tests') {
            steps {
                sh '''
                    cd backend
                    uv run pytest -v
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