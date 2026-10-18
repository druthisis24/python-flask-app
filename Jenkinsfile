
pipeline {
    agent any

    environment {
        VENV = '.venv'
        DEPLOY_DIR = "${WORKSPACE}/python-app-deploy"
    }

    stages {
        stage('Build') {
            steps {
                echo 'Creating virtual environment and installing dependencies'
                sh '''
                    python3 --version
                    python3 -m venv "$VENV"
                    "$VENV/bin/python" -m pip install --upgrade pip
                    "$VENV/bin/pip" install -r requirements.txt
                    "$VENV/bin/pip" install flake8
                '''
            }
        }

        stage('Code Quality') {
            steps {
                echo 'Checking Python code quality'
                sh '.venv/bin/flake8 app.py test_app.py --max-line-length=100'
            }
        }

        stage('Test') {
            steps {
                echo 'Running unit tests'
                sh '.venv/bin/python -m unittest discover -s . -p "test_*.py" -v'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Copying application to mock deployment directory'
                sh '''
                    mkdir -p "$DEPLOY_DIR"
                    cp app.py requirements.txt "$DEPLOY_DIR/"
                '''
            }
        }

        stage('Test Application') {
            steps {
                echo 'Starting the deployed application and testing HTTP response'
                sh '''
                    set -eu

                    .venv/bin/python "$DEPLOY_DIR/app.py" \
                        > "$DEPLOY_DIR/app.log" 2>&1 &
                    APP_PID=$!

                    cleanup() {
                        kill "$APP_PID" 2>/dev/null || true
                        wait "$APP_PID" 2>/dev/null || true
                    }
                    trap cleanup EXIT

                    READY=0
                    for i in $(seq 1 30); do
                        if ! kill -0 "$APP_PID" 2>/dev/null; then
                            cat "$DEPLOY_DIR/app.log"
                            exit 1
                        fi

                        if .venv/bin/python -c \
                            "import urllib.request; r=urllib.request.urlopen('http://127.0.0.1:5000/', timeout=2); assert r.status == 200 and r.read().decode() == 'Hello, Jenkins Multi-Stage Pipeline!'"
                        then
                            READY=1
                            break
                        fi

                        sleep 1
                    done

                    if [ "$READY" -ne 1 ]; then
                        cat "$DEPLOY_DIR/app.log"
                        echo "Application health check failed"
                        exit 1
                    fi

                    echo "Application HTTP health check passed"
                '''
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check the stage logs.'
        }
        always {
            echo 'Pipeline execution finished.'
        }
    }
}
