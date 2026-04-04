# DocuMind – AI Chatbot (Spring Boot + React)

A full-stack AI chatbot system built with a focus on **real-world backend engineering concerns**: streaming responses, vector search (RAG), authentication, and rate limiting.

---

## Features

### 1. Real-time Chat (SSE Streaming)

* Token-level streaming using Server-Sent Events (SSE)
* Non-blocking response delivery from backend to client
* Enables real-time interaction similar to ChatGPT

---

### 2. Conversation Memory

* Maintains contextual chat history per user
* Integrated via Spring AI `MessageChatMemoryAdvisor`
* Supports multi-turn conversations

---

### 3. RAG Pipeline (Retrieval-Augmented Generation)

* Document ingestion using Apache Tika (PDF, DOCX, TXT)
* Chunking and embedding generation (`text-embedding-3-small`)
* Stored in PostgreSQL using `pgvector`
* Semantic similarity search retrieves relevant context

---

### 4. Authentication & Authorization

* JWT-based authentication (access + refresh tokens)
* Spring Security filter chain for request validation
* User-scoped data isolation (conversations, documents)

---

### 5. Rate Limiting

* Sliding window rate limiter per user
* Protects chat endpoints from abuse
* Returns HTTP `429 Too Many Requests` with `Retry-After`

---

## System Design

### Architecture

```
Client (React)
      ↓
Spring Boot API
      ↓
PostgreSQL + pgvector
      ↓
OpenAI API
```

---

### Key Design Decisions

* **PostgreSQL + pgvector**

  * Avoids need for external vector DB
  * Simpler deployment and maintenance
  * Suitable for small–mid scale systems

* **SSE over WebSockets**

  * Simpler and stateless
  * Fits unidirectional streaming use-case
  * Lower overhead than WebSockets

* **JWT Authentication**

  * Stateless → enables horizontal scaling
  * No server-side session storage

---

## Tech Stack

| Layer    | Technology                          |
| -------- | ----------------------------------- |
| Backend  | Java 17, Spring Boot 3.4, Spring AI |
| AI       | OpenAI GPT-4o-mini                  |
| Database | PostgreSQL 16 + pgvector            |
| Auth     | JWT + Spring Security               |
| Frontend | React 18, TypeScript, Vite          |
| Infra    | Docker, Docker Compose, Flyway      |

---

## Getting Started

### 1. Clone Repository

```
git clone https://github.com/AbhishekU19/DocuMind.git
cd DocuMind
```

---

### 2. Setup Environment Variables

```
cp .env.example .env
```

Add your OpenAI API key:

```
OPENAI_API_KEY=your-api-key
```

---

### 3. Start Database

```
docker-compose -f docker-compose.dev.yml up -d
```

---

### 4. Run Backend

```
./mvnw spring-boot:run
```

Backend runs at:

```
http://localhost:8080
```

---

### 5. Run Frontend

```
cd frontend
npm install
npm run dev
```

Frontend runs at:

```
http://localhost:3000
```

---

### Alternative: Run Full Stack with Docker

```
docker-compose up --build
```

---

## API Documentation

Swagger UI:

```
http://localhost:8080/swagger-ui.html
```

### Key Endpoints

| Endpoint                | Method   | Description           |
| ----------------------- | -------- | --------------------- |
| `/api/auth/register`    | POST     | Create user           |
| `/api/auth/login`       | POST     | Authenticate user     |
| `/api/auth/refresh`     | POST     | Refresh token         |
| `/api/chat/{id}`        | POST     | Send message          |
| `/api/chat/{id}/stream` | GET      | Stream response (SSE) |
| `/api/documents/upload` | POST     | Upload document       |
| `/api/conversations`    | GET/POST | Manage conversations  |

---

## Project Structure

```
ai-chatbot-spring/
├── src/main/java/com/yourpackage/
│   ├── config/
│   ├── controller/
│   ├── dto/
│   ├── entity/
│   ├── repository/
│   ├── service/
│   ├── security/
│   └── exception/
├── frontend/
│   └── src/
│       ├── api/
│       ├── components/
│       ├── context/
│       ├── pages/
│       └── types/
├── docker-compose.yml
├── docker-compose.dev.yml
└── Dockerfile
```

---

## Scalability Considerations

* Stateless backend → horizontal scaling ready
* Vector search using pgvector indexing
* Rate limiting protects external AI API usage
* Can integrate Redis for:

  * caching
  * distributed rate limiting
  * session management (if needed)

---

## Future Improvements

* Redis caching layer
* Async document processing (queue-based)
* Multi-model support (Claude, Gemini)
* Observability (metrics + logging)
* WebSocket support for multi-user chat

---

## Author

Java Backend / Full Stack Developer

---
