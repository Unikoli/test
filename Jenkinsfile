pipeline {
    agent any

    environment {
	USER = "root"
        DEPLOY_SERVER = "root@192.168.30.53"
        APP_DIR = "/var/www/html"
    }

    stages {
        stage('Checkout') {
            steps {
		git changelog: false, poll: false, url: 'https://github.com/Unikoli/test.git'
		}
        }

        stage('Build') {
            steps {
                echo "Building the project..."
		sh """
			ssh ${USER}@${DEPLOY_SERVER} "ls -la ${APP_DIR}"
		"""
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

