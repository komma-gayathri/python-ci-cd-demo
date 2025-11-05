pipeline {
    agent any

    environment {
        APP_NAME = "python-ci-cd-demo"
        IMAGE_NAME = "gayathri/python-ci-cd-demo"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/komma-gayathri/python-ci-cd-demo.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t ${IMAGE_NAME} .'
            }
        }

        stage('Run Docker Container') {
            steps {
                sh 'docker run -d -p 8000:8000 ${IMAGE_NAME}'
            }
        }
    }

    post {
        always {
            echo "Pipeline finished."
        }
    }
}
