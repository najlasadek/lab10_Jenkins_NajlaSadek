pipeline {
    agent any
    environment {
        VIRTUAL_ENV = 'venv'
    }

    stages {

        stage('Setup') {
            steps {
                script {
                    if (!fileExists("${env.WORKSPACE}/${VIRTUAL_ENV}")) {
                        sh "python3 -m venv ${VIRTUAL_ENV}"
                    }
                    sh "source ${VIRTUAL_ENV}/bin/activate && pip install -r requirements.txt"
                }
            }
        }

        stage('Lint') {
            steps {
                sh "source ${VIRTUAL_ENV}/bin/activate && flake8 app.py"
            }
        }

        stage('Test') {
            steps {
                sh "source ${VIRTUAL_ENV}/bin/activate && pytest"
            }
        }

        stage('Coverage') {
            steps {
                sh "source ${VIRTUAL_ENV}/bin/activate && coverage run -m pytest"
                sh "source ${VIRTUAL_ENV}/bin/activate && coverage report -m"
            }
        }

        stage('Security Scan') {
            steps {
                sh "source ${VIRTUAL_ENV}/bin/activate && bandit -r ."
            }
        }

        stage('Deploy') {
            steps {
                sh "echo Deploying application..."
                sh "cp app.py /tmp/deployed_app.py"
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
