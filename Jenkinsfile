pipeline {
    agent any
    environment {
        VIRTUAL_ENV = 'venv'
    }

    stages {

        stage('Setup') {
            steps {
                script {
                    if (!fileExists("${env.WORKSPACE}\\${VIRTUAL_ENV}\\Scripts\\activate")) {
                        bat "python -m venv ${VIRTUAL_ENV}"
                    }
                    bat "${VIRTUAL_ENV}\\Scripts\\pip install -r requirements.txt"
                }
            }
        }

        stage('Lint') {
            steps {
                bat "${VIRTUAL_ENV}\\Scripts\\flake8 app.py"
            }
        }

        stage('Test') {
            steps {
                bat "${VIRTUAL_ENV}\\Scripts\\pytest"
            }
        }

        stage('Coverage') {
            steps {
                bat "${VIRTUAL_ENV}\\Scripts\\coverage run -m pytest"
                bat "${VIRTUAL_ENV}\\Scripts\\coverage report -m"
            }
        }

        stage('Security Scan') {
            steps {
                bat "${VIRTUAL_ENV}\\Scripts\\bandit -r ."
            }
        }

        stage('Deploy') {
            steps {
                bat "echo Deploying application..."
                bat "copy app.py C:\\temp\\deployed_app.py"
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
