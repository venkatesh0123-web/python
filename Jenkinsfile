pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/venkatesh0123-web/python.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh """
                python3 -m venv venv
                source venv/bin/activate
                pip install -r requirements.txt
                """
            }
        }

        stage('Run Tests') {
            steps {
                sh """
                source venv/bin/activate
                pytest --maxfail=1 --disable-warnings -q
                """
            }
        }

        stage('Run Application') {
            steps {
                sh """
                source venv/bin/activate
                python3 app.py
                """
            }
        }
    }
}
