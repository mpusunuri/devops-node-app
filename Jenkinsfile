pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE = 'devops-app'
        CONTAINER_NAME = 'devops-app-container'
    }
    
    stages {
        stage('Clone Repository') {
            steps {
                // CHANGE THIS LINE - replace YOUR_USERNAME with mpusunuri
                git branch: 'main', 
                    url: 'https://github.com/mpusunuri/devops-node-app.git'
            }
        }
        
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE}")
                }
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
    
    post {
        success {
            echo 'Pipeline executed successfully!'
            echo 'Application is running on port 3000'
        }
    }
}