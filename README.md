# tesis-chatbot

This project implements an automated evaluation pipeline for Large Language Models (LLMs) using:

-Retrieval-Augmented Generation (RAG)

Vector database (Qdrant)

Workflow orchestration with n8n

Safety filtering using Llama Guard

Multi-metric evaluation

The system is designed to analyze model performance under controlled retrieval and safety constraints.

⚙️ System Architecture

Data Source: PDF documents (Galápagos regulations)

Processing: Text extraction + chunking (500 / 200 overlap)

Storage: Qdrant vector database

Retrieval: Top-K semantic search

Generation: LLM (Gemini / GPT)

Evaluation: LLM-as-a-judge

Safety: Llama Guard filtering

🧱 Workflow

Load documents from Google Drive

Extract text from PDFs

Split into chunks

Generate embeddings

Store in Qdrant

Retrieve relevant chunks

Generate response

Evaluate response

Apply safety filter

📊 Metrics

Correctness

Helpfulness

Semantic similarity

Token usage

Execution time

📁 Dataset

The dataset consists of:

Domain-specific questions about Galápagos

Safety benchmark questions (S1–S13 categories)
