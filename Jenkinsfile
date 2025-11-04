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
                sh '
