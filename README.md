# Prashant AI Chatbot 🚀

A local AI chatbot built using **n8n**, **Ollama**, **Llama3**, **Docker**, and **Webhook APIs**.

This project demonstrates how to create a **local AI chatbot** using n8n workflows and Ollama. The chatbot runs locally, accepts user questions through a REST API, and generates responses using the **Llama3** model.

---

# 📌 Features

* AI chatbot powered by **Llama3**
* Custom AI identity (**"Prashant AI Chatbot"**)
* REST API using **n8n Webhook**
* Docker-based setup
* Local **Ollama** integration
* **Postman** and **curl** testing support
* Easy to extend with:

  * WhatsApp integration
  * Telegram bot
  * RAG / document chat
  * Memory support
  * Voice AI

---

# 🏗️ Architecture

```text
User Request
      ↓
n8n Webhook
      ↓
HTTP Request Node
      ↓
Ollama API (Llama3)
      ↓
Respond to Webhook
      ↓
API Response
```

---

# 🐳 Docker Setup

## Create `docker-compose.yml`

```yaml
version: '3'

services:
  n8n:
    image: n8nio/n8n
    ports:
      - "5678:5678"
    environment:
      - N8N_HOST=localhost
      - N8N_PORT=5678
      - N8N_PROTOCOL=http
    volumes:
      - ./n8n_data:/home/node/.n8n

  ollama:
    image: ollama/ollama
    ports:
      - "11434:11434"
    volumes:
      - ./ollama:/root/.ollama
```

---

# ▶️ Start Containers

Run the following command:

```bash
docker compose up -d
```

Verify running containers:

```bash
docker ps
```

---

# 🤖 Install Llama3

Enter the Ollama container:

```bash
docker exec -it <container_name> bash
```

Pull the Llama3 model:

```bash
ollama pull llama3
```

You can verify installed models:

```bash
ollama list
```

---

# 🌐 Access Applications

| Service    | URL                    |
| ---------- | ---------------------- |
| n8n        | http://localhost:5678  |
| Ollama API | http://localhost:11434 |

---

# 🔧 n8n Workflow Setup

## Workflow Nodes

```text
Webhook
   ↓
HTTP Request
   ↓
Respond to Webhook
```

---

## 1️⃣ Webhook Node Configuration

| Field          | Value                         |
| -------------- | ----------------------------- |
| HTTP Method    | POST                          |
| Path           | `prashant-ai-chatbot`         |
| Authentication | None                          |
| Respond        | Using Respond to Webhook Node |

---

## 2️⃣ HTTP Request Node Configuration

### Method

```text
POST
```

### URL

```text
http://host.docker.internal:11434/api/chat
```

### Body Content Type

```text
JSON
```

### JSON Body

```json
{
  "model": "llama3",
  "messages": [
    {
      "role": "system",
      "content": "You are Prashant AI Chatbot. Only when someone asks your name or who you are, reply with: I am Prashant AI Chatbot. For all other questions, answer normally and do not mention your name unless needed."
    },
    {
      "role": "user",
      "content": "{{$json.body.question}}"
    }
  ],
  "stream": false
}
```

---

## 3️⃣ Respond to Webhook Node

| Field        | Value               |
| ------------ | ------------------- |
| Respond With | First Incoming Item |

---

# 🚀 Publish Workflow

1. Save the workflow
2. Click **Publish**
3. Workflow becomes active

---

# 🧪 Test API Using curl

## Ask Name

```bash
curl --location 'http://localhost:5678/webhook/prashant-ai-chatbot' \
--header 'Content-Type: application/json' \
--data '{
  "question":"what is your name?"
}'
```

### Response

```json
{
  "model": "llama3",
  "message": {
    "role": "assistant",
    "content": "I am Prashant AI Chatbot."
  }
}
```

---

## Ask Normal Questions

```bash
curl --location 'http://localhost:5678/webhook/prashant-ai-chatbot' \
--header 'Content-Type: application/json' \
--data '{
  "question":"what is FastAPI?"
}'
```

---

# 📮 Test Using Postman

### Method

```text
POST
```

### URL

```text
http://localhost:5678/webhook/prashant-ai-chatbot
```

### Headers

```text
Content-Type: application/json
```

### Request Body

```json
{
  "question":"what is your name?"
}
```

---

# 📂 Example Workflow JSON

```json
{
  "nodes": [
    {
      "name": "Webhook"
    },
    {
      "name": "HTTP Request"
    },
    {
      "name": "Respond to Webhook"
    }
  ]
}
```

---

# ⚠️ Common Errors & Fixes

## ECONNREFUSED

### Cause

n8n cannot connect to Ollama.

### Fix

Use:

```text
http://host.docker.internal:11434/api/chat
```

Instead of:

```text
http://localhost:11434/api/chat
```

---

## Webhook Not Registered

### Cause

The workflow is not published or the test webhook has expired.

### Fix

* Click **Publish**
* Use the production webhook URL:

```text
/webhook/prashant-ai-chatbot
```

---

# 📈 Future Improvements

* Memory support
* RAG / document chat
* WhatsApp integration
* Telegram bot
* Voice AI
* Streaming responses
* OpenAI integration
* Public deployment
* Multi-model support

---

# 🛠️ Tech Stack

* n8n
* Ollama
* Llama3
* Docker
* REST API
* curl
* Postman

---

# 👨‍💻 Author

**Prashant AI Chatbot**

Built for showcasing **local AI chatbot development** using **n8n + Ollama + Llama3**.
