# 🚀 Deploying Authentik IDP with Docker & NGINX Proxy Manager

This guide provides a step-by-step approach to deploying **Authentik IDP** using **Docker & NGINX Proxy Manager (NPM)** on an **Ubuntu server**.

---

## 🏗️ **Prerequisites**
Before proceeding, ensure you have:
- A domain name (e.g., `authentik.goskope.com`)
- An **AWS Elastic IP Address** (or a static IP)
- An **Ubuntu Server** with at least:
  - **2 CPU Cores**
  - **2GB RAM**
  - Open inbound **ports 22, 80, 443, 81** (Optionally **9000 & 9443** for direct access)

---

## 📍 **Step 1: Configure DNS**
Create an **A record** for your IDP domain and point it to your **AWS Elastic IP**.

Example:
authentik.goskope.com → YOUR_ELASTIC_IP


---

## 🏗️ **Step 2: Set Up Ubuntu Server in AWS**
1. Launch an **Ubuntu Server (22.04 LTS recommended)**.
2. Configure **security rules** to allow the following ports:
   - **22** (SSH)
   - **80, 443** (Web traffic)
   - **81** (NPM admin panel)
   - *(Optionally allow 9000 & 9443 for direct access)*

---

## 🐳 **Step 3: Install Docker & Docker Compose**
Please refer following links: 

Docker Installation Guide - https://docs.docker.com/engine/install/ubuntu/
Post-installation steps - https://docs.docker.com/engine/install/linux-postinstall/

## 🐳 **Step 4: Download docker-compose.yml**

Clone the GitHub repository and navigate to the directory:
git clone https://github.com/kumarsecurityfocal/authentik-iam.git
cd authentik-iam

## 🐳 **Step 5: Generate Credentials**

echo "PG_PASS=$(openssl rand -base64 36 | tr -d '\n')" >> .env
echo "AUTHENTIK_SECRET_KEY=$(openssl rand -base64 60 | tr -d '\n')" >> .env

To enable error reporting:
echo "AUTHENTIK_ERROR_REPORTING__ENABLED=true" | tee -a .env

## 🐳 **Step 6: Configure SMTP (Optional but recommended)**

AUTHENTIK_EMAIL__HOST=localhost
AUTHENTIK_EMAIL__PORT=25
AUTHENTIK_EMAIL__USERNAME=
AUTHENTIK_EMAIL__PASSWORD=
AUTHENTIK_EMAIL__USE_TLS=false
AUTHENTIK_EMAIL__USE_SSL=false
AUTHENTIK_EMAIL__TIMEOUT=10
AUTHENTIK_EMAIL__FROM=authentik@localhost

(Modify the values according to your SMTP settings.)

## 🐳 **Step 7: Append following lines in .env file**
This is primarily to define postgresql credentials and for adding containers for NGINX & NGINX Proxy Manager (NPM)

PG_USER=authentik
PG_PASS=supersecurepassword
PG_DB=authentik

AUTHENTIK_IMAGE=ghcr.io/goauthentik/server
AUTHENTIK_TAG=2024.12.3

NPM_DB_HOST=npm-db
NPM_DB_PORT=3306
NPM_DB_NAME=npm
NPM_DB_USER=npm_user
NPM_DB_PASSWORD=npm_password

NPM_HTTP_PORT=80
NPM_HTTPS_PORT=443
NPM_UI_PORT=81



## 🐳 **Step 8: Deploy the containers**

docker compose up -d
Verify the services are running:
docker ps

## 🌐 **Step 9: Set Up Reverse Proxy in NGINX Proxy Manager**

Access NPM Admin Panel:
Open http://<your-server-ip>:81 in your browser.
Log in using NPM default credentials (admin@example.com / changeme).
Add a new proxy host:
Domain Name: authentik.goskope.com
Forward Hostname/IP: server
Forward Port: 9000
Enable Websockets & SSL

## 🎉 **Step 10: Complete Authentik Setup**

http://<your-server-ip>/if/flow/initial-setup/
Set up an admin username & password.
Log in and start configuring Authentik IDP!

## 🔥 **Step 11: Troubleshooting**
 
Check logs:
docker compose logs -f

Restart a service:

docker compose restart <service-name>
If Authentik isn’t accessible, ensure NPM is correctly forwarding requests.

🎯 **Conclusion**
You’ve successfully deployed Authentik IDP using Docker & NGINX Proxy Manager! 🎉

🚀 Need help? Check out the Authentik Docs or open an issue on this repository.

📢 **Contributions**
Feel free to fork and contribute to enhance this project!

🛠 **Author**
Security Focal

🔹 Happy Deploying! 🚀
---









