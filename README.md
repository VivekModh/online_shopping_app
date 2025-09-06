# Online Shop App 🛒

A full-stack **E-commerce Web Application** built to demonstrate containerization, CI/CD, and deployment using **Docker** and **Jenkins**. The app allows users to browse products, add items to a cart, and place orders, simulating a real-world online store workflow.

---


1. Home Page
![Home Page](public/homePage.png)
1. Admin Page
![Admin Page](public/adminPage.png)

---

## Project Features
- Browse and search products
- Add/remove products from the shopping cart
- Place orders and view order history
- Containerized deployment for scalability
- Fully automated CI/CD pipeline using Jenkins
- Security scanning with Trivy

---

## Tech Stack
- **Frontend/Backend:** Node.js, npm, optionally Python (pip) or Java (Maven)  
- **Containerization & Orchestration:** Docker, Docker Compose  
- **CI/CD:** Jenkins  
- **Security:** Trivy  
- **Networking:** Docker networks  

---

## 🚀 Getting Started

### 1. Clone the Project
```bash
git clone https://github.com/VivekModh/online_shopping_app.git
cd online_shopping_app

2. Build Docker Image
docker build -t online-shop-app:latest .
docker images   # Verify the image

3. Run Docker Container
docker run -d -p 5173:5173 online-shop-app:latest
docker ps       # Check running containers
docker logs <container_id>   # Check logs

4. Persist Container Data Using Volumes
docker run -d -v /home/ubuntu/volume/online_shop:/logs -p 5173:5173 online-shop-app:latest

5. Docker Networks
Docker supports 7 types of networks; most commonly used:
none
host
bridge
user-defined bridge
Create a custom network:
docker network create my-net
Run container on custom network:
docker run -d --network my-net --name nginx -p 80:80 nginx:latest

6. Multi-Container Management (Docker Compose)
docker-compose up -d
docker-compose down


⚙️ Jenkins CI/CD Pipeline
Setup:
Create DockerHub Personal Access Token

Add credentials in Jenkins → dockerHubCred

Configure pipeline using the provided Jenkinsfile

Use Ngrok (if running Jenkins on WSL) to expose GitHub Webhook:
ngrok http 8080
Add Webhook in GitHub Repo Settings:

Payload URL: https://<ngrok-url>/github-webhook/

Content type: application/json

Enable SSL verification

Select Send me everything
