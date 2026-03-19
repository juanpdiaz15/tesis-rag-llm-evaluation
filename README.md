# tesis-chatbot

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

1. Requisitos

- VPS (Ubuntu 20.04+ recomendado)
- Docker
- Docker Compose

2. Install Docker and Docker Compose
First, connect to your VPS via SSH. Next, install Docker and Docker Compose. The official convenience script is the easiest way to get the latest Docker version. Verify both installations with docker --version and docker compose version.
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
