pipeline {
    agent any

    environment {
        VENV = 'venv'
    }

    stages {
        stage('Clone') {
            steps {
                git 'https://github.com/Angad0691996/basic-CICD.git'
            }
        }

        stage('Set Up Environment') {
            steps {
                sh 'python3 -m venv $VENV'
                sh './$VENV/bin/pip install --upgrade pip'
                sh './$VENV/bin/pip install -r requirements.txt'
            }
        }

        stage('Run Tests') {
            steps {
                sh './$VENV/bin/python -m pytest test_app.py'
            }
        }

        stage('Deploy') {
            steps {
                // Kill existing Gunicorn if running
                sh 'pkill gunicorn || true'

                // Run app in background using Gunicorn
                sh './$VENV/bin/gunicorn -w 2 -b 0.0.0.0:5000 app:app &'
            }
        }
    }

    post {
        failure {
            echo "❌ Build failed. Check the console output for errors."
        }
        success {
            echo "✅ Successfully deployed the Flask app!"
        }
    }
}
