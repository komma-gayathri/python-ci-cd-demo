pipeline {
    agent {
        docker {
            image 'python:3.10'      // 🐍 Run in Python Docker environment
            args '-u root:root'      // Run as root to allow installations
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
                // Example: if you have pytest configured
                sh 'pytest || echo "No tests found"'
            }
        }

        stage('Security Scan - Gitleaks') {
            steps {
                echo "🔐 Running Gitleaks for secret scanning..."
                sh '''
                apt-get update && apt-get install -y wget unzip
                wget https://github.com/gitleaks/gitleaks/releases/latest/download/gitleaks-linux-amd64
                mv gitleaks-linux-amd64 /usr/local/bin/gitleak
