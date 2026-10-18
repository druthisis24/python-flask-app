# Python Flask Application – Jenkins Multi-Stage Pipeline

## Project Overview

This project demonstrates Continuous Integration (CI) and a basic deployment workflow using **Jenkins**, **Python Flask**, **Git**, and **GitHub**.

A Flask web application is integrated with a Jenkins Declarative Pipeline that automates dependency installation, code quality checks, unit testing, deployment, and application verification.

## Technologies Used

- **Python** – Application development
- **Flask** – Web application framework
- **Jenkins** – CI/CD pipeline automation
- **Git and GitHub** – Version control and source code management
- **flake8** – Python code quality checking
- **unittest** – Automated unit testing

## Project Structure

```text
python-flask-app/
├── app.py
├── test_app.py
├── requirements.txt
├── Jenkinsfile
├── Dockerfile.jenkins
├── .gitignore
├── README.md
├── jenkins success one.png
└── jenkins success two.png
```

## Application Details

The Flask application runs on port `5000` and provides a home endpoint (`/`) that returns:

```text
Hello, Jenkins Multi-Stage Pipeline!
```

## Jenkins Pipeline Stages

The `Jenkinsfile` defines the following stages:

1. **Build** – Creates a Python virtual environment, installs the project dependencies, and installs flake8.
2. **Code Quality** – Checks Python files using flake8.
3. **Test** – Executes automated unit tests using Python's unittest framework.
4. **Deploy** – Copies the application files into a deployment directory in the Jenkins workspace.
5. **Test Application** – Starts the deployed Flask application and verifies its HTTP response.

## Setup and Execution

### Run the application locally

Install the dependencies:

```bash
python -m pip install -r requirements.txt
```

Start the application:

```bash
python app.py
```

Open the following URL in your browser:

```text
http://localhost:5000
```

### Run the unit tests

```bash
python -m unittest discover -v
```

### Run the code quality check

```bash
python -m pip install flake8
python -m flake8 app.py test_app.py --max-line-length=100
```

## Jenkins Configuration

1. Open the Jenkins dashboard.
2. Create or open a Pipeline job.
3. Select **Pipeline script from SCM**.
4. Select **Git** as the SCM.
5. Enter the repository URL.
6. Set the branch specifier to `*/main`.
7. Set the Script Path to `Jenkinsfile`.
8. Save the configuration and click **Build Now**.

The Jenkins execution environment must have Python 3, Git, and the required virtual environment support installed.

## Successful Build Screenshots

### Jenkins Success – Screenshot 1

![Jenkins successful pipeline - screenshot one](jenkins%20success%20one.png)

### Jenkins Success – Screenshot 2

![Jenkins successful pipeline - screenshot two](jenkins%20success%20two.png)

## Expected Outcome

When all stages pass, Jenkins reports a successful build. This demonstrates automated source checkout, dependency installation, code quality validation, unit testing, deployment to a workspace directory, and application response verification.

**Note:** The Deploy stage copies files into a deployment directory in the Jenkins workspace. This project demonstrates a basic pipeline deployment workflow, not production infrastructure deployment.

## Author

Developed as a Python Flask and Jenkins CI/CD pipeline project.
