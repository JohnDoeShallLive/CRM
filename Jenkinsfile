pipeline {
    agent any

    environment {
        GIT_CREDENTIALS = 'github-token'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git credentialsId: "${GIT_CREDENTIALS}", url: 'https://github.com/JohnDoeShallLive/CRM.git', branch: 'main'
            }
        }

        stage('Build Application') {
            steps {
                echo "Building the Frappe CRM application..."
                sh 'echo Build complete!'  // Replace with actual build commands
            }
        }

        stage('Run Tests') {
            steps {
                echo "Running tests..."
                sh 'echo Tests passed!'  // Replace with actual test commands
            }
        }

        stage('Deploy Application') {
            steps {
                echo "Deploying Frappe CRM..."
                sh 'echo Deployment successful!'  // Replace with actual deployment steps
            }
        }
    }
}
