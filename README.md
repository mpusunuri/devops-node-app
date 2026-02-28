# 🚀 DevOps Task: Node.js + Docker + Jenkins CI/CD Pipeline

![DevOps](https://img.shields.io/badge/DevOps-Complete-brightgreen)
![Docker](https://img.shields.io/badge/Docker-Containerized-blue)
![Jenkins](https://img.shields.io/badge/Jenkins-CI%2FCD-orange)
![Node.js](https://img.shields.io/badge/Node.js-Express-green)
![GitHub](https://img.shields.io/badge/GitHub-Webhook-purple)

## 📋 Project Overview
This project demonstrates a complete DevOps workflow by containerizing a Node.js application and implementing a fully automated CI/CD pipeline using Jenkins with GitHub webhook integration.

### 🎯 Objective
Build a simple Node.js application, containerize it using Docker, and create a Jenkins pipeline that automatically builds and deploys whenever code is pushed to GitHub.

## 🌐 Live Application
Access the running application at: http://13.233.182.144:3000


## 📁 Project Structure 

devops-node-app/
│
├── 📄 server.js # Express application (main server file)
├── 📄 package.json # Node.js dependencies and scripts
├── 📄 Dockerfile # Docker configuration for containerization
├── 📄 .dockerignore # Files to exclude from Docker build
├── 📄 Jenkinsfile # CI/CD pipeline definition with webhook
└── 📄 README.md # Project documentation (this file)



## 🛠️ Technologies Used
| Technology | Purpose |
|------------|---------|
| **Node.js** | JavaScript runtime for backend |
| **Express.js** | Web framework for Node.js |
| **Docker** | Containerization platform |
| **Jenkins** | CI/CD automation server |
| **GitHub** | Version control and repository |
| **AWS EC2** | Cloud hosting platform |
| **GitHub Webhook** | Auto-trigger Jenkins on push |

## 📦 Application Details

### server.js
```javascript
const express = require("express");
const app = express();

app.get("/", (req, res) => {
  res.send("DevOps Task Running");
});

const PORT = 3000;

app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});


///package.json

{
  "name": "devops-task-app",
  "version": "1.0.0",
  "description": "Simple Express app for DevOps task",
  "main": "server.js",
  "scripts": {
    "start": "node server.js"
  },
  "dependencies": {
    "express": "^4.18.2"
  }
}

///docker file

FROM node:18-alpine
WORKDIR /usr/src/app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 3000
CMD ["npm", "start"]

///.ignore 

node_modules
npm-debug.log
.git
.gitignore
README.md
.dockerignore
Jenkinsfile

🔧 Setup Instructions
Prerequisites
✅ Node.js installed (v18 or higher)

✅ Docker installed

✅ GitHub account

✅ AWS EC2 instance with Jenkins

✅ MobaXterm (for EC2 connection)

Local Development Setup
bash
# Clone the repository
git clone https://github.com/mpusunuri/devops-node-app.git
cd devops-node-app

# Install dependencies
npm install

# Run locally
node server.js
# Visit http://localhost:3000

🔄 Jenkins Pipeline
Jenkinsfile
groovy
pipeline {
    agent any
    
    triggers {
        // GitHub webhook trigger - auto-starts on git push
        githubPush()
    }
    
    environment {
        DOCKER_IMAGE = 'devops-app'
        CONTAINER_NAME = 'devops-app-container'
    }
    
    stages {
        stage('Clone Repository') {
            steps {
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


📊 Pipeline Stages Explanation
Stage	Description	Commands Used
1. Clone Repository	Pulls latest code from GitHub	#git clone
2. Install Dependencies	Installs Node.js dependencies	#npm install
3. Build Docker Image	Creates Docker image	docker build
4. Run Docker Container	Deploys the application	docker run
🌐 GitHub Webhook Trigger (Bonus Feature)
This project includes automatic CI/CD with GitHub webhooks!

How Webhook Works
Developer pushes code to GitHub
GitHub sends webhook to Jenkins
Jenkins auto-starts pipeline
New container deploys updated app
Changes go live automatically!

Webhook Configuration
text
Payload URL: http://13.233.182.144:8080/github-webhook/
Content type: application/json
Events: Just the push event
Active: ✓

🐳 Docker Commands Used
Basic Commands
bash
# Build Docker image
docker build -t devops-app .

# Run container
docker run -d -p 3000:3000 --name devops-app-container devops-app

# List running containers
docker ps

# View logs
docker logs devops-app-container

# Stop container
docker stop devops-app-container

# Remove container
docker rm devops-app-container

Verification Commands
bash
# Check if app is running
curl http://localhost:3000

# Check container status
docker ps | grep devops-app

# Check image
docker images | grep devops-app
🔍 Verification Steps
After Pipeline Success:
Check Docker container:

bash
docker ps
Test the application:

bash
curl http://localhost:3000
# Expected: DevOps Task Running
Open in browser:
http://13.233.182.144:3000

⚙️ CI/CD Workflow Diagram

┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│  Developer  │────▶│   GitHub    │────▶│   Webhook   │
│  git push   │     │ Repository  │     │   Trigger   │
└─────────────┘     └─────────────┘     └─────────────┘
                                                  │
                                                  ▼
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│   Browser   │◀────│     EC2     │◀────│   Jenkins   │
│  View App   │     │  Container  │     │ Auto-Build  │
└─────────────┘     └─────────────┘     └─────────────┘

📸 Screenshots

Screenshot 1: Running Docker Container
text
$ docker ps
CONTAINER ID   IMAGE                 COMMAND                  CREATED          STATUS          PORTS                              NAMES
cf0b974e41c0   devops-app            "docker-entrypoint.s…"   2 minutes ago    Up 2 minutes    0.0.0.0:3000->3000/tcp             devops-app-container
cff3c183dad6   jenkins/jenkins:lts   "/usr/bin/tini -- /u…"   2 hours ago      Up 2 hours      0.0.0.0:8080->8080/tcp             jenkins

$ curl http://localhost:3000

DevOps Task Running
Screenshot 2: Successful Jenkins Build
text
Started by user devops
Obtained Jenkinsfile from git https://github.com/mpusunuri/devops-node-app.git
[Pipeline] Start of Pipeline
...
[Pipeline] stage
[Pipeline] { (Run Docker Container)
[Pipeline] script
[Pipeline] {
[Pipeline] sh
+ docker run -d --name devops-app-container -p 3000:3000 devops-app
cf0b974e41c0346cd63a9d432140c0f8f348ef7fdc06c265755933b5fcba6898
[Pipeline] }
[Pipeline] // script
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (Declarative: Post Actions)
[Pipeline] echo
Pipeline executed successfully!
[Pipeline] echo
Application is running on port 3000
[Pipeline] }
[Pipeline] // stage
[Pipeline] End of Pipeline
Finished: SUCCESS
Screenshot 3: Browser View
text
DevOps Task Running

⚠️ Troubleshooting Guide
Issue	Solution
Port 3000 in use	sudo lsof -i :3000 && sudo kill -9 <PID>
Docker permission denied	sudo usermod -aG docker $USER && newgrp docker
npm not found in Jenkins	Install Node.js in Jenkins container
Webhook not triggering	Check GitHub webhook deliveries for errors
Container exits immediately	docker logs devops-app-container to see errors
💰 AWS Cost Management
To avoid charges when not using:

bash
# Stop EC2 instance
sudo shutdown -h now
Or from AWS Console: EC2 → Instances → Stop

✅ Task Completion Checklist
Node.js Express app created

Dockerfile configured

.dockerignore created

Jenkinsfile with all stages

GitHub repository with all files

Successful Jenkins build

Docker container running on EC2

Application accessible via browser

GitHub webhook configured

Auto-deployment working on git push

Complete README documentation

🎉 Key Achievements
✓ Complete CI/CD Pipeline - From code push to auto-deployment
✓ Docker Containerization - Consistent environment
✓ Jenkins Automation - No manual builds needed
✓ GitHub Webhook - Auto-trigger on every push
✓ Cloud Deployment - Running on AWS EC2

👨‍💻 Author

mpusunuri
GitHub: @mpusunuri
Project: devops-node-app

📅 Project Timeline
Created: February 28, 2026

Last Updated: February 28, 2026

Status: ✅ Completed with Bonus Webhook Feature

📝 How to Test the Complete Pipeline
Step 1: Clone the Repository
bash
git clone https://github.com/mpusunuri/devops-node-app.git
cd devops-node-app
Step 2: Make a Change
bash
echo "// Testing auto-deploy" >> server.js
Step 3: Push to GitHub
bash
git add .
git commit -m "Test auto-deployment"
git push
Step 4: Watch Magic Happen! ✨
GitHub webhook triggers Jenkins

Jenkins auto-builds

New container deploys

Your changes go live in seconds!

Step 5: Verify
bash
curl http://13.233.182.144:3000
# Your updated app is live!
🌟 Final Note
This project successfully demonstrates all required DevOps concepts:

Docker fundamentals (images vs containers)

CI/CD concepts (continuous integration and deployment)

Jenkins pipeline structure (declarative syntax)

GitHub integration (webhooks and automation)

Thank you for reviewing my DevOps task!


For any questions or issues, please open an issue on GitHub.

