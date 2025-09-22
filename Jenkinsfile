pipeline {
    agent any

    environment {
        DEPLOY_SERVER = "root@192.168.30.53"
        APP_DIR = "/var/www/html"
        GIT_REPO = "git@github.com:Unikoli/test.git"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: "${GIT_REPO}"
            }
        }

        stage('Build') {
            steps {
                echo "Building the project..."
            }
        }

        stage('Test') {
            steps {
                echo "Running tests..."
            }
        }

        stage('Deploy') {
            steps {
                echo "Deploying to Webserver..."
            }
        }
    }

    post {
        success {
            echo "Deployment Successful ✅"
        }
        failure {
            echo "Deployment Failed ❌"
        }
    }
}

