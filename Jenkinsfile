pipeline {
    agent any

    environment {
        DEPLOY_USER = 'root'                  // or a sudo user
        DEPLOY_HOST = '192.168.30.53'
        APP_PATH    = '/var/www/htmlsite'
        SITE_NAME   = 'htmlsite'
    }

    stages {
	stage('Git checkout') {
	steps {
	git changelog: false, poll: false, url: 'https://github.com/Unikoli/test.git'
}
}
        stage('Build') {
            steps {
                sh 'echo "Building ......."'
            }
        }

        stage('Deploy') {
            steps {
                sshagent (credentials: ['server']) {
                    sh """
                        # Create target directory
                        ssh $DEPLOY_USER@$DEPLOY_HOST "mkdir -p $APP_PATH"

                        # Sync HTML files
                        rsync -avz --delete ./ $DEPLOY_USER@$DEPLOY_HOST:$APP_PATH/
                    """
                }
            }
        }

        stage('Configure Nginx') {
            steps {
                sshagent (credentials: ['server']) {
                    sh """
                        ssh $DEPLOY_USER@$DEPLOY_HOST '
                            cat > /etc/nginx/conf.d/$SITE_NAME <<EOF
server {
    listen 80;
    server_name 192.168.30.53;
    root $APP_PATH;
    index index.html;

    location / {
        try_files \$uri \$uri/ =404;
    }
}
EOF

                            ln -sf /etc/nginx/conf.d/$SITE_NAME /etc/nginx/conf.d/$SITE_NAME
                            nginx -t
                            systemctl reload nginx
                        '
                    """
                }
            }
        }
    }

    post {
        success { echo "✅ Site deployed and Nginx configured successfully." }
        failure { echo "❌ Deployment or configuration failed. Check logs." }
    }
}

