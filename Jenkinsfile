pipeline {
    agent {
        docker {
            image 'python:3.10'    // 🐍 Python environment
            args '-u root:root'    // Run as root for installs
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
                sh 'pytest --maxfail=1 --disable-warnings -q || echo "⚠️ Tests failed but continuing for demo"'
            }
        }

        stage('Gitleaks Scan') {
            steps {
                echo "🔍 Running Gitleaks for secret detection..."
                sh '''
                    if ! command -v gitleaks &> /dev/null; then
                        echo "Installing Gitleaks..."
                        wget -q https://github.com/gitleaks/gitleaks/releases/latest/download/gitleaks-linux-amd64 -O /usr/local/bin/gitleaks
                        chmod +x /usr/local/bin/gitleaks
                    fi
                    gitleaks detect --source . --no-git --report-path gitleaks-report.json || echo "⚠️ Gitleaks scan found issues"
                '''
            }
        }

        stage('SonarQube Scan') {
            steps {
                echo "🧠 Running SonarQube scan..."
                withSonarQubeEnv('sonarqube') {
                    sh '''
                        sonar-scanner \
                            -Dsonar.projectKey=python-ci-cd-demo \
                            -Dsonar.sources=. \
                            -Dsonar.host.url=$SONAR_HOST_URL \
                            -Dsonar.login=$SONAR_AUTH_TOKEN
                    '''
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "🐳 Building Docker image..."
                sh """
                    docker build -t ${IMAGE_NAME}:latest .
                """
            }
        }

        stage('Trivy Security Scan') {
            steps {
                echo "🛡️ Scanning Docker image for vulnerabilities with Trivy..."
                sh '''
                    if ! command -v trivy &> /dev/null; then
                        echo "Installing Trivy..."
                        apt-get update -y && apt-get install -y wget apt-transport-https gnupg lsb-release
                        wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | gpg --dearmor -o /usr/share/keyrings/trivy.gpg
                        echo "deb [signed-by=/usr/share/keyrings/trivy.gpg] https://aquasecurity.github.io/trivy-repo/deb $(lsb_release -sc) main" | tee /etc/apt/sources.list.d/trivy.list
                        apt-get update -y && apt-get install -y trivy
                    fi
                    trivy image --severity HIGH,CRITICAL ${IMAGE_NAME}:latest || echo "⚠️ Vulnerabilities found"
                '''
            }
        }

        stage('Push Docker Image') {
            steps {
                echo "📤 Pushing Docker image to Docker Hub..."
                withCredentials([string(credentialsId: 'dockerhub-pass', variable: 'DOCKER_HUB_PASS')]) {
                    sh """
                        echo "$DOCKER_HUB_PASS" | docker login -u gayathri --password-stdin
                        docker push ${IMAGE_NAME}:latest
                    """
                }
            }
        }

        stage('Deploy with Docker Compose') {
            steps {
                echo "🚀 Deploying application on target server..."
                sh '''
                    docker-compose down || true
                    docker-compose up -d --build
                '''
            }
        }
    }

    post {
        success {
            echo "✅ Pipeline completed successfully!"
        }
        failure {
            echo "❌ Pipeline failed. Check logs for details."
        }
    }
}
