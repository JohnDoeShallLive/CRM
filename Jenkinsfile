pipeline {
    agent any

    environment {
        GIT_CREDENTIALS = 'github-token'  // Set up Jenkins credentials for GitHub
        FRAPPE_SITE_NAME = 'mysite.local'
        DB_PASSWORD = 'admin'  // Change this to a secure password
        NODE_VERSION = '18'
    }

    stages {
        stage('Clone Repository') {
            steps {
                echo "🔹 Cloning repository..."
                git credentialsId: "${GIT_CREDENTIALS}", url: 'https://github.com/JohnDoeShallLive/CRM.git', branch: 'develop'
            }
        }

        stage('Install Dependencies') {
        steps {
            echo "🔹 Installing required dependencies..."
            sh '''
            echo "shreyash@123" | sudo -S apt update && sudo -S apt install -y \
            python3-pip python3-dev python3-venv \
            mariadb-server mariadb-client \
            redis-server xvfb libfontconfig \
            wkhtmltopdf curl nodejs npm yarn
            '''
        }
    }


        stage('Setup MySQL Database') {
            steps {
                echo "🔹 Setting up MySQL Database..."
                sh '''
                sudo systemctl start mariadb
                sudo mysql -e "CREATE DATABASE frappe_db;"
                sudo mysql -e "CREATE USER 'frappe'@'localhost' IDENTIFIED BY 'frappe_password';"
                sudo mysql -e "GRANT ALL PRIVILEGES ON frappe_db.* TO 'frappe'@'localhost';"
                sudo mysql -e "FLUSH PRIVILEGES;"
                '''
            }
        }

        stage('Install Frappe Bench') {
            steps {
                echo "🔹 Installing Frappe Bench..."
                sh '''
                pip3 install frappe-bench
                bench init --frappe-branch version-14 frappe-bench
                '''
            }
        }

        stage('Create New Frappe Site') {
            steps {
                echo "🔹 Creating new Frappe site..."
                sh '''
                cd frappe-bench
                bench new-site ${FRAPPE_SITE_NAME} --db-name=frappe_db --mariadb-root-password=${DB_PASSWORD} --admin-password=admin --install-app erpnext
                '''
            }
        }

        stage('Install CRM Application') {
            steps {
                echo "🔹 Installing Frappe CRM App..."
                sh '''
                cd frappe-bench
                bench get-app https://github.com/JohnDoeShallLive/CRM.git
                bench --site ${FRAPPE_SITE_NAME} install-app CRM
                '''
            }
        }

        stage('Run Frappe Server') {
            steps {
                echo "🔹 Starting Frappe Server..."
                sh '''
                cd frappe-bench
                nohup bench start > frappe.log 2>&1 &
                '''
            }
        }

        stage('Run Tests') {
            steps {
                echo "🔹 Running tests..."
                sh '''
                cd frappe-bench
                bench --site ${FRAPPE_SITE_NAME} run-tests
                '''
            }
        }

        stage('Deploy to Production') {
            steps {
                echo "🔹 Deploying Application..."
                sh '''
                echo " Deployment steps go here, such as setting up Nginx, Supervisor, or Docker"
                '''
            }
        }
    }

    post {
        success {
            echo " Build and Deployment Successful!"
        }
        failure {
            echo " Build Failed. Check logs!"
        }
    }
}
