pipeline {
    agent any
    environment {
        DOCKER_IMAGE = "gayathri/python-ci-cd-demo"
    }
    stages {
        stage('Checkout Code') {
            steps {
                // Use the Git plugin
                git branch: 'main', url: 'https://github.com/komma-gayathri/python-ci-cd-demo.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Remove old image if exists
                    sh 'docker rm -f python-ci-cd-demo || true'
                    sh 'docker rmi -f $DOCKER_IMAGE || true'
                    
                    // Build new Docker image
                    sh 'docker build -t $DOCKER_IMAGE .'
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    // Stop old container if running
                    sh 'docker stop python-ci-cd-demo || true'
                    sh 'docker rm -f python-ci-cd-demo || true'

                    // Run new container
                    sh 'docker run -d --name python-ci-cd-demo -p 8000:8000 $DOCKER_IMAGE'
                }
            }
        }
    }

    post {
        failure {
            echo "Pipeline failed! Check console output for errors."
        }
        success {
            echo "Pipeline succeeded! Application is running on port 8000."
        }
    }
}
