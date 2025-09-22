pipeline {
    agent any

    environment {
        DEPLOY_USER = 'root'
        DEPLOY_HOST = '192.168.30.53'
        APP_PATH    = '/var/www/html'
    }

    stages {
        stage('Build') {
            steps {
                // Optional: simple check or minify step
                sh 'echo "Build stage: nothing to compile for plain HTML"'
            }
        }

        stage('Test') {
            steps {
                // Simple HTML syntax check (skip if not needed)
                sh 'grep -qi "<html" index.html || (echo "No <html> tag!" && exit 1)'
            }
        }

        stage('Deploy') {
            steps {
                sshagent (credentials: ['server']) {
                    sh """
                        rsync -avz --delete ./ $DEPLOY_USER@$DEPLOY_HOST:$APP_PATH/
                    """
                }
            }
        }
    }

    post {
        success { echo "✅ HTML deployed successfully." }
        failure { echo "❌ Deployment failed. Check logs." }
    }
}

