pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Bootstrap Docker (First Run Only)') {
            steps {
                sh '''
                if ! command -v docker >/dev/null 2>&1; then
                    echo "Docker not found. Installing Docker..."
                    sudo dnf install -y docker
                    sudo systemctl daemon-reexec
                    sudo systemctl enable docker
                    sudo systemctl start docker
                    sudo usermod -aG docker jenkins
                    echo "Docker installed. PLEASE RE-RUN THE JOB."
                    exit 2
                else
                    echo "Docker already installed. Skipping bootstrap."
                fi
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t jenkins-nginx .'
            }
        }

        stage('Run Docker Container') {
            steps {
                sh '''
                docker stop nginx-container || true
                docker rm nginx-container || true
                docker run -d -p 8081:80 --name nginx-container jenkins-nginx
                '''
            }
        }
    }
}
