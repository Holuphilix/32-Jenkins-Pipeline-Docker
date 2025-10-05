# 🧩Mini Project: Jenkins Pipeline with Docker Integration

## 🚀 Project Overview

This mini project demonstrates a **Continuous Integration and Continuous Deployment (CI/CD)** pipeline using **Jenkins** integrated with **Docker**.
The pipeline automates the **build**, **test**, and **deployment** stages of a containerized web application using **Jenkins Declarative Pipeline syntax**.

## 🧱 Prerequisites

Ensure the following components are installed and configured before proceeding:

* **Jenkins** (with Pipeline plugin)
* **Docker** (installed on the Jenkins host)
* **GitHub account** (for source control and webhook integration)
* **Ubuntu/Linux** environment

## 📂 Project Directory Structure

```bash
jenkins-pipeline-docker/
│
├── docker.sh                # Shell script for installing and configuring Docker
├── Dockerfile               # Docker configuration for building NGINX container
├── Jenkinsfile              # Jenkins declarative pipeline definition
├── index.html               # Sample static web application file
└── README.md                # Project documentation
```

> 💡 The `docker.sh` script simplifies Docker installation on your Jenkins instance.
> The `Dockerfile` and `index.html` form the web application that the pipeline builds and deploys.

## ⚙️ Installation & Setup

### 1️⃣ Docker Installation via Shell Script

Before running the pipeline, install Docker on your Jenkins instance using the provided `docker.sh` script:

```bash
chmod +x docker.sh
./docker.sh
```

This script:

* Updates the system package list
* Installs dependencies and Docker’s official GPG key
* Adds the Docker repository
* Installs Docker Engine, CLI, and Compose plugin
* Checks Docker service status

Output when successful:

```
● docker.service - Docker Application Container Engine
   Loaded: loaded (/lib/systemd/system/docker.service; enabled; vendor preset: enabled)
   Active: active (running)
```

### 2️⃣ Jenkins Pipeline Setup

#### 🧩 Pipeline Configuration

1. Open Jenkins and create a **new Pipeline job**.
2. Under **Build Triggers**, configure:

   * *GitHub webhook integration* for automatic builds on push.
3. Under **Pipeline**, choose “Pipeline script from SCM” if using GitHub, or paste the script directly.

#### 🕹️ Jenkinsfile

```groovy
pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building Docker image...'
                sh 'docker build -t jenkins-docker-demo .'
            }
        }

        stage('Test') {
            steps {
                echo 'Running basic container test...'
                sh 'docker run -d --name test-container -p 8080:80 jenkins-docker-demo'
                sh 'sleep 5'
                sh 'curl -I http://localhost:8080'
                sh 'docker stop test-container && docker rm test-container'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying container...'
                sh 'docker run -d -p 80:80 --name nginx-container jenkins-docker-demo'
            }
        }
    }

    post {
        success {
            echo 'Pipeline executed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check the logs for details.'
        }
    }
}
```

### 3️⃣ Docker Configuration

#### 🧾 Dockerfile

```dockerfile
# Use the official NGINX base image
FROM nginx:latest

# Set the working directory in the container
WORKDIR /usr/share/nginx/html/

# Copy the local HTML file to the NGINX default public directory
COPY index.html /usr/share/nginx/html/

# Expose port 80 to allow external access
EXPOSE 80
```

#### 💻 index.html

```html
Congratulations, You have successfully run your first pipeline code.
```

This builds a lightweight **NGINX** container serving a simple web page as a deployment target for the Jenkins pipeline.

## 🔗 GitHub Integration

* Connect your GitHub repository to Jenkins via **webhook** (`Settings → Webhooks`).
* Automated builds trigger on **push** or **pull request** events.
* Jenkins reports build status back to GitHub.

## 🌟 Key Features

* Automated Jenkins CI/CD pipeline
* Shell script for Docker setup
* Containerized NGINX web application
* GitHub webhook integration
* Declarative pipeline syntax for readability

## 🧭 Best Practices

1. Use **Declarative Pipelines** for clarity and modularity.
2. Implement **error handling** and build **notifications**.
3. Use **Docker image tags** for version control.
4. Store secrets securely using Jenkins Credentials Manager.
5. Regularly back up your Jenkins configurations.

## 🧰 Troubleshooting

| Issue                    | Possible Solution                                      |
| ------------------------ | ------------------------------------------------------ |
| Docker permission denied | Add Jenkins user to `docker` group and restart Jenkins |
| Build not triggered      | Check GitHub webhook and Jenkins job configuration     |
| Pipeline failure         | Review Jenkins console output for errors               |
| Image not building       | Validate `Dockerfile` syntax and file path             |
| Webpage not accessible   | Ensure port mapping (`-p 80:80`) is correct            |

## 🤝 Contributing

Contributions are welcome!

1. **Fork** this repository
2. **Create a feature branch** (`git checkout -b feature-name`)
3. **Commit your changes** (`git commit -m "Add new feature"`)
4. **Push** to your branch (`git push origin feature-name`)
5. **Open a Pull Request**

## 👨‍💻 Author

**Philip Oluwaseyi Oludolamu**
*DevOps Engineer | Cloud & Automation Enthusiast*

📧 **Email:** [oluphilix@gmail.com](mailto:oluphilix@gmail.com)
🌐 **GitHub:** [@Holuphilix](https://github.com/Holuphilix)
💼 **LinkedIn:** [Philip Oludolamu](https://www.linkedin.com/in/philip-oludolamu)

