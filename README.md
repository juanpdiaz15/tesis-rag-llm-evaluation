# Tesis-rag-llm-evaluation

This project implements an automated evaluation pipeline for Large Language Models (LLMs) using:

- Retrieval-Augmented Generation (RAG)

- Vector database (Qdrant)

- Workflow orchestration with n8n

- Safety filtering using Llama Guard

- Multi-metric evaluation

The system is designed to analyze model performance under controlled retrieval and safety constraints.

## ⚙️ System Architecture

1. Data Source: PDF documents (Galápagos regulations)

2. Processing: Text extraction + chunking (500 / 200 overlap)

3. Storage: Qdrant vector database

4. Retrieval: Top-K semantic search

5. Generation: LLM (Gemini / GPT)

6. Evaluation: LLM-as-a-judge

7. Safety: Llama Guard filtering

## 🤖 Model Access

All models used in this project are accessed via external APIs and are not hosted locally. The system integrates with:

- OpenAI (GPT models)
- Google Cloud (Gemini models)
- Groq (inference for open-source models)

All API calls are handled internally within the n8n workflows. To run the project, each user must configure their own API keys inside n8n. This approach allows easy model swapping and avoids the need for local model deployment.


## 📊 Metrics

- Correctness

- Helpfulness

- Semantic similarity

- Token usage

- Execution time

## 📁 Dataset

The dataset consists of:

- Domain-specific questions about Galápagos

- Safety benchmark questions (S1–S13 categories)

## Deployment Guide (VPS + Docker)

1. Requierements

- VPS (Ubuntu 20.04+ recomended)
- Docker
- Docker Compose

2. Install Docker and Docker Compose

   First, connect to your VPS via SSH. Next, install Docker and Docker Compose. The official convenience script is the easiest way to get the latest Docker version.    Verify both installations with docker --version and docker compose version.
```bash
sudo apt update && sudo apt upgrade -y
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
sudo apt install docker-compose-v2 -y
```

3. Create the n8n Configuration
   
   Organize your setup by creating a dedicated directory for n8n.
```bash
mkdir ~/n8n && cd ~/n8n
```
   Inside this directory, create two files: docker-compose.yml for the service definition and .env for your secret credentials.

   Create the docker-compose.yml file:
```bash
nano docker-compose.yml
```
   Paste or upload the docker-compose.yml file

   Now, create the .env file to securely store your database credentials.
```bash
nano .env
```
   Paste or upload the .env-example file

4. Start n8n
   
   With the configuration in place, you can install n8n and start the services with a single command.
```bash
docker compose up -d
```
   This command downloads the required Docker images and starts the containers in the background. You can check the status with docker compose ps. At this point, n8n is running and ready to be secured for production use. It's a good idea to check the logs once (docker compose logs -f n8n) to ensure there were no errors during startup.

## Securing Your n8n Instance
1.  Install and Configure NGINX

   First, install NGINX on your VPS.
```bash
sudo apt install nginx -y
```
   Next, create a new NGINX server block configuration file for your n8n domain. Make sure your domain's DNS A record is already pointing to your server's IP address.
```bash
sudo nano /etc/nginx/sites-available/n8n
```
   Paste in the following configuration, replacing n8n.yourdomain.com with your actual domain. The settings included are optimized for n8n's use of Server-Sent Events (SSE).
```bash
server {
    listen 80;
    server_name n8n.yourdomain.com;

    location / {
        proxy_pass http://localhost:5678;
        proxy_set_header Connection '';
        proxy_http_version 1.1;
        chunked_transfer_encoding off;
        proxy_buffering off;
        proxy_cache off;
        proxy_set_header Host $host;
    }
}
```
   Then test and restart NGINX.
```bash
sudo ln -s /etc/nginx/sites-available/n8n /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl restart nginx
```
2. Enable HTTPS with Let's Encrypt
   
   Install Certbot and its NGINX plugin:
```bash
sudo apt install certbot python3-certbot-nginx -y
```
   Run Certbot to obtain and install the certificate. It will automatically detect your domain from the NGINX configuration and guide you through the process.
```bash
sudo certbot --nginx -d n8n.yourdomain.com
```
Follow the prompts, making sure to select the option to redirect all HTTP traffic to your n8n HTTPS. Once finished, Certbot will automatically handle certificate renewals. You can now access your secure n8n instance at https://n8n.yourdomain.com.

## Citation
```bash
@thesis{diaz2025rag,
  title={Comparative Evaluation of Vulnerabilities in LLMs},
  author={Diaz Castillo, Juan Pablo},
  year={2025}
}
```
