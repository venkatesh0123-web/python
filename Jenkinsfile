pipeline {
    agent any

    environment {
        IMAGE_NAME = "venkatesh0123-web/python-jenkins-demo"
        IMAGE_TAG = "latest"
        REGISTRY_CREDENTIALS = "dockerhub-creds"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/venkatesh0123-web/python.git'
            }
        }

        stage('Setup Python & Install Dependencies') {
            steps {
                sh """
                python3 -m venv venv
                source venv/bin/activate
                pip install --upgrade pip
                pip install -r requirements.txt
                """
            }
        }

        stage('Run Tests') {
            steps {
                sh """
                source venv/bin/activate
                pytest --junitxml=pytest-report.xml
                """
            }
            post {
                always {
                    junit 'pytest-report.xml'
                }
            }
        }

        stage('Run Application') {
            steps {
                sh """
                source venv/bin/activate
                python3 app.py > output.txt
                """
            }
        }

        stage('Archive Build Artifacts') {
            steps {
                archiveArtifacts artifacts: 'output.txt', followSymlinks: false
            }
        }

        stage('Docker Build & Push') {
            steps {
                script {
                    docker.withRegistry('', REGISTRY_CREDENTIALS) {
                        def app = docker.build("${venkatesh0123-web/python-jenkins-demo}:${v1}")
                        app.push()
                    }
                }
            }
        }
    }

    post {
        success {
            echo "🎉 Build completed successfully!"
        }
        failure {
            echo "❌ Build failed. Check logs."
        }
    }
}
