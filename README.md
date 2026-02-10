# 🦈 ImageShark: Cloud-Native Image Sharing Platform

**ImageShark** is a real-time, room-based image sharing application built with Python (Flask) and deployed as a scalable microservice on Microsoft Azure.

This project demonstrates a complete **DevOps lifecycle**: containerizing a full-stack application, pushing to a cloud registry, orchestrating via Kubernetes (AKS), and connecting to a managed PostgreSQL database.

---

## 🏗️ Architecture & Tech Stack

This project creates a fully decoupled cloud architecture where the application logic is stateless (running in containers) and data is persistent (managed database).

| Component | Technology | Description |
| :--- | :--- | :--- |
| **Frontend** | HTML5, Jinja2, CSS3 | Server-side rendered templates for dynamic content. |
| **Backend** | Python 3.9, Flask | RESTful routes handling Auth, Room Logic, and QR Generation. |
| **Database** | Azure PostgreSQL (Flex Server) | Managed relational database for Users, Sessions, and Rooms. |
| **Containerization** | Docker | Multi-stage builds using `python:3.9-slim` images. |
| **Orchestration** | Azure Kubernetes Service (AKS) | Managed Kubernetes cluster for high availability and scaling. |
| **Registry** | Azure Container Registry (ACR) | Private storage for Docker images (`imagesharkreg99`). |
| **Infrastructure** | Azure Resource Group | Logical grouping of all cloud resources. |

---

## 📸 Deployment Validation

### 1. Cloud Infrastructure (Azure)
All resources were provisioned in the **Sweden Central** region under a unified Resource Group (`ImageShark-RG`). This includes the AKS Cluster, the Container Registry, and the Virtual Network.
<img src="Azure%20Resource%20Group.png" alt="Azure Resource Group">

The database is hosted on a managed **Azure Database for PostgreSQL (Flexible Server)**, ensuring high availability and automated backups.
<img src="Azure%20DB%20Overview.png" alt="Azure DB Overview">

### 2. Containerization Workflow (Docker & ACR)
The application was developed locally using Docker Desktop to ensure environment consistency before cloud deployment.
<img src="docker%20desktop.png" alt="Docker Desktop Local Images">

Versioning is handled via Docker Tags (`v1` -> `v6`). Images are pushed to the **Azure Container Registry (ACR)**, where Kubernetes pulls the specific version defined in the deployment manifest.
<img src="Container%20Registry%20Versions.png" alt="Container Registry Versions">

### 3. Kubernetes Orchestration Status
The app runs on a Linux-based AKS Agent Pool. The service is exposed via a **LoadBalancer** with a public IP address.
* **Status:** Running (1/1 Replicas)
* **External Access:** Configured via LoadBalancer Service
* **Self-Healing:** Pods automatically restart if failure occurs.

*Below: Live terminal output showing the External-IP assignment and healthy pod status.*
<img src="Kubectl%20Get%20Nodes%20Status.png" alt="Kubectl Get Nodes Status">

### 4. Database Implementation
Data persistence is secured and structured.
* **Schema:** Normalized tables for `Users`, `Rooms`, and `Sessions`.
* **Tooling:** Verified using DBeaver connected securely to the Azure instance.
<img src="Database%20Schema%20in%20DBeaver.png" alt="Database Schema in DBeaver">

---

## 🚀 Live Application Demo

The application was successfully accessible via the public internet.

### Landing Page & UI
Users are greeted with a responsive landing page where they can create new rooms or join existing ones.
<br>
<img src="Landing%20Page.jpg" width="800" alt="Landing Page">

*User Dashboard view:*
<br>
<img src="Room%20&%20QR%20Code%20Feature%20PC.jpg" width="800" alt="Room Dashboard">

### Core Feature: Room Sharing
The app dynamically generates **QR codes** for easy mobile joining. Users can instantly connect by scanning the code.
<br>
<img src="Room%20&%20QR%20Code%20Feature.png" width="800" alt="QR Code Modal">

### Mobile Responsiveness
The application is fully optimized for mobile devices, allowing users to confirm their identity and join rooms on the go.
<br>
<img src="Room%20&%20QR%20Code%20Feature%20phone.jpg" width="350" alt="Mobile View">

---

## 🛠️ Local Development Setup

To run this application locally without Kubernetes:

1.  **Clone the repository:**
    ```bash
    git clone [https://github.com/YOUR_USERNAME/ImageShark-Cloud-Native.git](https://github.com/YOUR_USERNAME/ImageShark-Cloud-Native.git)
    cd ImageShark/src
    ```

2.  **Set up Virtual Environment:**
    ```bash
    python -m venv venv
    source venv/bin/activate  # On Windows: venv\Scripts\activate
    ```

3.  **Install Dependencies:**
    ```bash
    pip install -r requirements.txt
    ```

4.  **Configure Environment:**
    Create a `.env` file with your database credentials:
    ```env
    DB_HOST=localhost
    DB_NAME=postgres
    DB_USER=your_local_user
    DB_PASS=your_local_password
    ```

5.  **Run the App:**
    ```bash
    python app.py
    ```

---

## ☁️ Cloud Deployment Steps (Azure AKS)

These are the commands used to deploy the live version:

1.  **Build the Docker Image:**
    ```bash
    docker build -t imagesharkreg99.azurecr.io/imageshark:v7 .
    ```

2.  **Push to Azure Registry:**
    ```bash
    docker push imagesharkreg99.azurecr.io/imageshark:v7
    ```

3.  **Apply Kubernetes Manifest:**
    ```bash
    kubectl apply -f deployment.yaml
    ```

4.  **Monitor Deployment:**
    ```bash
    kubectl get services --watch
    ```

---
*Author: Luchian Marco-Antonio*
