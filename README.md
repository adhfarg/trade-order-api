# Trade Order API

## Overview
This project is a simple backend service built using **FastAPI** and **PostgreSQL**, designed to handle trade orders. The application is containerized using **Docker** and is deployed on **AWS EC2** with a **CI/CD pipeline using GitHub Actions**.

## Features
✅ Accepts trade orders via `POST /orders`  
✅ Retrieves a list of submitted orders via `GET /orders`  
✅ Stores order data in **PostgreSQL**  
✅ Containerized using **Docker** & **Docker Compose**  
✅ Deployed on **AWS EC2**  
✅ CI/CD pipeline using **GitHub Actions**  
✅ API documentation available via **Swagger UI**  

---

## 📌 API Endpoints

### **1️⃣ Create an Order**
**Endpoint:** `POST /orders`

#### **Request Body**
```json
{
    "product_name": "Sample Product",
    "price": 29.99,
    "quantity": 2,
    "symbol": "AAPL",
    "order_type": "buy"
}
```
#### **Response**
```json
{
    "symbol": "AAPL",
    "price": 29.99,
    "quantity": 2,
    "order_type": "buy",
    "id": 1
}
```

---

### **2️⃣ Get All Orders**
**Endpoint:** `GET /orders`

#### **Response**
```json
[
    {
        "symbol": "AAPL",
        "price": 29.99,
        "quantity": 2,
        "order_type": "buy",
        "id": 1
    }
]
```

---

## 🚀 Running the Application Locally

### **1️⃣ Clone the Repository**
```bash
git clone (followed with https of git hub)
cd trade-order-api
```

### **2️⃣ Run with Docker Compose**
```bash
docker-compose up --build
```

### **3️⃣ Access FastAPI Swagger UI**
Go to:  
👉 `http://127.0.0.1:8000/docs`

---

## 🐳 Docker Setup

### **Dockerfile**
```dockerfile
FROM python:3.10
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000"]
```

### **Docker Compose Configuration** (`docker-compose.yml`)
```yaml
version: "3.8"
services:
  db:
    image: postgres:15-alpine
    container_name: postgres_db
    restart: always
    environment:
      POSTGRES_USER: user
      POSTGRES_PASSWORD: password
      POSTGRES_DB: trade_db
    ports:
      - "5432:5432"
    volumes:
      - postgres_data:/var/lib/postgresql/data

  app:
    build: .
    container_name: fastapi_app
    ports:
      - "8000:8000"
    depends_on:
      - db
    environment:
      DATABASE_URL: postgresql://user:password@db:5432/trade_db

volumes:
  postgres_data:
```

---

## ☁️ Deploying on AWS EC2

### **1️⃣ Launch an EC2 Instance**
- Use **Ubuntu 20.04 or later**
- Install **Docker & Docker Compose**

### **2️⃣ Connect via SSH**
```bash
ssh -i your-key.pem ubuntu@your-ec2-instance-ip
```

### **3️⃣ Clone the Repository on EC2**
```bash
git clone (copy https from git hub)
cd trade-order-api
```

### **4️⃣ Run Docker on EC2**
```bash
docker-compose up --build -d
```

---

## 🔄 CI/CD Pipeline (GitHub Actions)
This project includes a **GitHub Actions** workflow to automate:
- Running **tests on PRs**
- **Building the Docker image**
- **Deploying to AWS EC2**

### **GitHub Actions Workflow (`.github/workflows/deploy.yml`)**
```yaml
name: Deploy to EC2

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v2
      
      - name: SSH into EC2 and deploy
        uses: appleboy/ssh-action@master
        with:
          host: ${{ secrets.EC2_HOST }}
          username: ubuntu
          key: ${{ secrets.EC2_SSH_KEY }}
          script: |
            cd trade-order-api
            git pull origin main
            docker-compose down
            docker-compose up --build -d
```

---

## 🎥 Loom Video Walkthrough
A full walkthrough of setting up, running, and testing the API is available here: **[Loom Video Link]**

---

## 📜 License
This project is open-source and available for learning and development purposes.

---
