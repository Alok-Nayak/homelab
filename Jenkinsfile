// Jenkinsfile
pipeline {
    agent any
    stages {
        stage('Checkout Code') {
            steps {
                echo 'Pulling source code from Git..'
                git 'https://github.com/Alok-Nayak/homelab.git'
            }
        }

        stage('Build & Test') {
            steps {
                echo 'Building and testing the application using the builder container...'
                sh 'docker exec builder npm install'
                sh 'docker exec builder npm run build'
                sh 'docker exec builder npm run test'
            }
        }

        stage('Archive Artifacts') {
            steps {
                echo 'Archiving build artifacts to the artifact server...'
                sh 'docker cp $(docker ps -qf "name=builder"):/app/dist/index.html artifact_server:/usr/share/nginx/html/index.html'
            }
        }

        stage('Deploy to Nginx') {
            steps {
                echo 'Deploying the built files to the main Nginx container...'
                sh 'docker cp $(docker ps -qf "name=builder"):/app/dist/index.html nginx:/usr/share/nginx/html/'
            }
        }

        stage('Notify') {
            steps {
                echo 'Notifying team of successful deployment...'
                sh 'echo "Deployment complete."'
            }
        }
    }
}

