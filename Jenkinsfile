pipeline {
    agent any
    environment {
        DOCKER_IMAGE = "gayathri/python-ci-cd-demo"
    }
    stages {
        stage('Checkout Code') {
            steps {
                // Use the Git plugin directly
                git branch: 'main', url: 'https://github.com/komma-gayathri/python-ci-cd-demo.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }
        stage('Run Docker Container') {
            steps {
                sh 'docker run -d -p 8000:8000 $DOCKER_IMAGE'
            }
        }
    }
}
