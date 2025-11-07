pipeline {
    agent any
    environment {
        PYTHON = "C:\\Users\\sadek\\AppData\\Local\\Programs\\Python\\Python310\\python.exe"
        VENV = "venv"
    }

    stages {

        stage('Setup') {
            steps {
                script {
                    // Create virtual environment if it does not exist
                    if (!fileExists("${env.WORKSPACE}\\${VENV}\\Scripts\\activate.bat")) {
                        bat "\"${PYTHON}\" -m venv ${VENV}"
                    }
                    // Install dependencies
                    bat "\"${env.WORKSPACE}\\${VENV}\\Scripts\\python.exe\" -m pip install --upgrade pip"
                    bat "\"${env.WORKSPACE}\\${VENV}\\Scripts\\python.exe\" -m pip install -r requirements.txt"
                }
            }
        }

        stage('Lint') {
            steps {
                bat "\"${env.WORKSPACE}\\${VENV}\\Scripts\\python.exe\" -m flake8 app.py"
            }
        }

        stage('Test') {
            steps {
                bat "\"${env.WORKSPACE}\\${VENV}\\Scripts\\python.exe\" -m pytest"
            }
        }

        stage('Coverage') {
            steps {
                bat "\"${env.WORKSPACE}\\${VENV}\\Scripts\\python.exe\" -m coverage run -m pytest"
                bat "\"${env.WORKSPACE}\\${VENV}\\Scripts\\python.exe\" -m coverage report -m"
            }
        }

           stage('Security Scan') {
        steps {
            bat '@chcp 65001 > nul & "venv\\Scripts\\python.exe" -m bandit -r . -f txt -o bandit_report.txt'
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
