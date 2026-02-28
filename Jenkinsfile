pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE = 'devops-app'
        CONTAINER_NAME = 'devops-app-container'
    }
    
    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', 
                    url: 'https://github.com/YOUR_USERNAME/devops-node-app.git'
            }
        }
        
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t ${DOCKER_IMAGE} .'
            }
        }
        
        stage('Run Docker Container') {
            steps {
                script {
                    sh '''
                        docker stop ${CONTAINER_NAME} || true
                        docker rm ${CONTAINER_NAME} || true
                        docker run -d --name ${CONTAINER_NAME} -p 3000:3000 ${DOCKER_IMAGE}
                    '''
                }
            }
        }
    }
}