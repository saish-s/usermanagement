pipeline {
    agent any
    environment {
        DOCKER_HUB_USER = 'saishshindepersistent'
    }
    stages {
        stage('Clone') {
            steps {
                git branch: 'master', url: 'https://github.com/saish-s/usermanagement.git'
            }
        }
        stage('Build Maven') {
            steps {
                sh 'mvn clean package -DskipTests'
            }    
        }
        stage('Docker Build & Push') {
            steps {
                script {
                    def appImage = docker.build("${DOCKER_HUB_USER}/usermanagement:latest")
                    docker.withRegistry('', 'docker-hub-credentials') {
                        appImage.push()
                    }
                }
            }
        }
        stage('Deploy with Compose') {
            steps {
                sh 'docker compose up -d'
                sh 'sleep 20'
                sh 'docker ps'
            }
        }
        stage('Cleanup') {
            steps {
                sh 'rm -rf *'
            }
        }
    }
}
