pipeline {
    agent any

    stages {

        stage('Install Docker') {
            steps {
                sh '''
                if ! command -v docker &> /dev/null
                then
                    sudo dnf install docker -y
                    sudo systemctl start docker
                    sudo systemctl enable docker
                    sudo usermod -aG docker jenkins
                else
                    echo "Docker already installed"
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
