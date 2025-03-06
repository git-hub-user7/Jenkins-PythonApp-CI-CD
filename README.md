# Jenkins CI/CD Pipeline for Python Flask Application  

**Automated Dockerization, Testing, and Deployment**  

---

## 📝 Project Description  
This project demonstrates a **CI/CD pipeline** using Jenkins to automate the Dockerization, testing, and deployment of a Python Flask web application. Key features:  
- **GitHub Integration**: Auto-trigger Jenkins pipelines on code commits.  
- **Docker Image Management**: Build and push images to Docker Hub.  
- **Unit Testing**: Run `pytest` to validate application functionality.  
- **Windows Compatibility**: Configured for seamless execution on Windows environments.  

---

## 🛠 Prerequisites  
1. **Python 3.12+** (installed with `PATH` configuration).  
2. **Docker Desktop** (Windows/Mac).  
3. **Jenkins** (installed locally).  
4. **Accounts**:  
   - GitHub ([Sign up](https://github.com))  
   - Docker Hub ([Sign up](https://hub.docker.com))  

---

## 🚀 Step-by-Step Guide  

### 1. **Clone the Repository**  
```bash
git clone https://github.com/<your-github-username>/Jenkins-PythonApp-CI-CD.git
cd Jenkins-PythonApp-CI-CD
```
2. **Project Structure**
```   
├── app/
│   ├── app.py          # Flask application
│   ├── test_app.py     # Unit tests
│   └── requirements.txt
├── Dockerfile          # Docker configuration
└── Jenkinsfile         # Jenkins pipeline script
```
3. **Docker Setup**
Build the Docker Image:
```
docker build -t <your-dockerhub-username>/python-app:latest .
```
Push to Docker Hub:
```
docker login
docker push <your-dockerhub-username>/python-app:latest
```
4. **Jenkins Pipeline Configuration**
Install Plugins:

GitHub Integration

Docker Pipeline

Blue Ocean (optional)

Create a Pipeline Job:

Name: `python-app-pipeline`

GitHub Repo URL: `https://github.com/<your-github-username>/Jenkins-PythonApp-CI-CD.git`

Branch: `main`

Script Path: `Jenkinsfile`

Add Credentials to Jenkins:

Docker Hub: Username + Personal Access Token (ID: `dockerhub-creds`).

GitHub: Personal Access Token (ID: `github-creds`).

5. **Jenkinsfile**
```
pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-creds')
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', 
                url: 'https://github.com/<your-github-username>/Jenkins-PythonApp-CI-CD.git'
            }
        }
        stage('Test') {
            steps {
                bat 'pytest app/test_app.py' // Windows-compatible
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("<your-dockerhub-username>/python-app:\${env.BUILD_ID}")
                }
            }
        }
        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub-creds') {
                        dockerImage.push()
                    }
                }
            }
        }
    }
}
```
6. **Automate with GitHub Webhook**
Go to GitHub Repo → Settings → Webhooks.

Add a webhook with:

Payload URL: `http://<your-jenkins-ip>:8080/github-webhook/`

Trigger: Just the push event

📸 **Screenshots** 
images/jenkins-pipeline-success.png - Successful Jenkins pipeline stages.

images/dockerhub-repository.png - Docker Hub repository with pushed image.

images/pytest-results.png - Unit test results in Jenkins console.

images/github-webhook.png - GitHub webhook configuration.

🔧 **Troubleshooting**
Issue                     	Solution
Authentication failed----------Use Docker Hub/GitHub Personal Access Tokens.
sh: command not found----------Replace sh with bat in Jenkinsfile.
Docker push denied-------------Verify credentials in Jenkins.
pytest not found---------------Install pytest globally: pip install pytest.

🖥 Technologies Used
📦Jenkins (CI/CD Automation)

📦Docker (Containerization)

📦Python 3.12 (Flask Web Framework)

📦GitHub (Version Control & Webhooks)
