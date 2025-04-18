pipeline {
    agent any

    environment {
        VENV = 'venv'
    }

    stages {
        stage('Clone') {
            steps {
                echo '✔️ Code is already checked out by Jenkins (SCM: main branch)'
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

                // Run app in background using Gunicorn and save output to log
                sh 'nohup ./venv/bin/gunicorn -w 2 -b 0.0.0.0:5000 app:app > gunicorn.log 2>&1 &'
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
