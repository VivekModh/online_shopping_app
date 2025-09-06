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

## Getting Started

### 1. Clone the Project
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
Create custom network:
docker network create my-net
Run container on custom network:
docker run -d --network my-net --name nginx -p 80:80 nginx:latest
6. Multi-Container Management
docker-compose up -d
docker-compose down

Jenkins CI/CD Pipeline
Setup:

Create DockerHub personal access token

Add credentials in Jenkins (dockerHubCred)

Trigger pipeline automatically via GitHub webhook

Use Ngrok to expose local WSL Ubuntu server:
ngrok http 8080
GitHub webhook URL: https://<ngrok-id>.ngrok-free.app/github-webhook/
