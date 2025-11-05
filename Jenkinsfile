pipeline {
    agent {
        docker {
            image 'python:3.10'   // 🐍 Python environment inside Docker
            args '-u root:root'   // Run as root for installs
        }
    }

    environment {
        APP_NAME = "python-ci-cd-demo"
        IMAGE_NAME = "gayathri/python-ci-cd-demo"
    }

    stages {

        stage('Checkout Code') {
            steps {
                echo "📥 Checking out code from GitHub..."
                git branch: 'main', url: 'https://github.com/komma-gayathri/python-ci-cd-demo.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                echo "📦 Installing dependencies..."
                sh 'pip install -r requirements.txt'
            }
        }

        stage('Run Tests') {
            steps {
                echo "🧪 Running tests..."
                sh '''
                echo "No tests yet — skipping for now."
                '''
            }
        }

        stage('Security Scan - Gitleaks') {
            steps {
                echo "🔐 Running Gitleaks for secret scanning..."
                sh '''
                apt-get update && apt-get install -y wget unzip
                wget https://github.com/gitleaks/gitleaks/releases/latest/download/gitleaks-linux-amd64
                mv gitleaks-linux-amd64 /usr/local/bin/gitleaks
                chmod +x /usr/local/bin/gitleaks
                gitleaks detect --source . --no-banner || true
                '''
            }
        }

        stage('Security Scan - Trivy') {
            steps {
                echo "🛡️ Scanning Docker image for vulnerabilities..."
                sh '''
                apt-get install -y curl
                curl -sfL https://raw.githubusercontent.com/aquasecurity/trivy/main/contrib/install.sh | sh
                ./bin/trivy fs --exit-code 0 --no-progress .
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "🐳 Building Docker image..."
                sh "docker build -t
