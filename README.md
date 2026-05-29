# Prashant AI Agent 🚀

An AI chatbot built using:

- **n8n**
- **Ollama**
- **Llama3**
- **Docker**
- **Webhook APIs**
- **curl / Postman testing**

This project demonstrates how to create a local AI agent using n8n workflows and Ollama.

---

# 📌 Features

- AI chatbot using Llama3
- Custom AI identity ("Prashant AI Agent")
- REST API using n8n Webhook
- Docker-based setup
- Local Ollama integration
- Postman & curl testing
- Easy to extend with WhatsApp, Telegram, RAG, etc.

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

## Create docker-compose.yml

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

```bash
docker compose up -d
```

---

# 🤖 Install Llama3

Enter Ollama container:

```bash
docker exec -it <container_name> bash
```

Pull model:

```bash
ollama pull llama3
```

---

# 🌐 Access Applications

| Service | URL |
|---|---|
| n8n | http://localhost:5678 |
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

# 1️⃣ Webhook Node Configuration

| Field | Value |
|---|---|
| HTTP Method | POST |
| Path | prashant-ai-agent |
| Authentication | None |
| Respond | Using Respond to Webhook Node |

---

# 2️⃣ HTTP Request Node Configuration

## Method

```text
POST
```

## URL

```text
http://host.docker.internal:11434/api/chat
```

## Body Content Type

```text
JSON
```

## JSON Body

```json
{
  "model": "llama3",
  "messages": [
    {
      "role": "system",
      "content": "You are Prashant AI Agent. Only when someone asks your name or asks who you are, reply with: I am Prashant AI Agent. For all other questions, answer normally and do not mention your name unless needed."
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

# 3️⃣ Respond to Webhook Node

| Field | Value |
|---|---|
| Respond With | First Incoming Item |

---

# 🚀 Publish Workflow

1. Save workflow
2. Click Publish
3. Workflow becomes active

---

# 🧪 Test API Using curl

## Ask Name

```bash
curl --location 'http://localhost:5678/webhook/prashant-ai-agent' \
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
    "content": "I am Prashant AI Agent."
  }
}
```

---

# 🧪 Test Normal Questions

```bash
curl --location 'http://localhost:5678/webhook/prashant-ai-agent' \
--header 'Content-Type: application/json' \
--data '{
  "question":"what is FastAPI?"
}'
```

---

# 📮 Test Using Postman

## Method

```text
POST
```

## URL

```text
http://localhost:5678/webhook/prashant-ai-agent
```

## Headers

```text
Content-Type: application/json
```

## Body

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

instead of:

```text
http://localhost:11434/api/chat
```

---

## Webhook Not Registered

### Cause

Workflow not published or webhook test expired.

### Fix

- Click Publish
- Use production URL:

```text
/webhook/prashant-ai-agent
```

---

# 📈 Future Improvements

- Memory support
- RAG / document chat
- WhatsApp integration
- Telegram bot
- Voice AI
- Streaming responses
- OpenAI integration
- Public deployment

---

# 🛠️ Tech Stack

- n8n
- Ollama
- Llama3
- Docker
- REST API
- curl
- Postman

---

# 👨‍💻 Author

**Prashant AI Agent Project**

Built for showcasing local AI automation using n8n + Ollama.

