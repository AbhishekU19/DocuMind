# DocuMind - AI Chatbot Spring Boot + React

A production-ready AI chatbot that actually works. Built with Spring Boot 3.4, Spring AI, and React — not another copy-paste tutorial project.

This is v1.0: a solid foundation you can extend. Real JWT auth, real SSE streaming (not the fake kind that splits text by spaces), real RAG with pgvector, and a clean React UI with dark mode.

## What It Does

- **Chat with AI** — OpenAI GPT integration with genuine Server-Sent Events streaming. You see tokens appear in real-time, just like ChatGPT.
- **Conversation Memory** — Spring AI's `MessageChatMemoryAdvisor` handles context automatically. The AI remembers what you talked about.
- **RAG (Retrieval-Augmented Generation)** — Upload PDFs, DOCX, or text files. The system extracts text via Apache Tika, chunks it, generates embeddings, and stores them in PostgreSQL + pgvector. When you ask a question, relevant context is pulled from your documents.
- **JWT Authentication** — Register, login, refresh tokens. Each user has their own conversations and documents.
- **Rate Limiting** — Per-user sliding window rate limiter on chat endpoints. Returns proper `429` with `Retry-After` headers.

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Backend | Java 17, Spring Boot 3.4.3, Spring AI 1.0.0-M5 |
| AI | OpenAI GPT-4o-mini, text-embedding-3-small |
| Database | PostgreSQL 16 + pgvector |
| Auth | JWT (jjwt 0.12.6), Spring Security |
| Frontend | React 18, TypeScript, Vite 6, Tailwind CSS 4 |
| Docs | SpringDoc OpenAPI (Swagger UI) |
| Infra | Docker, Docker Compose, Flyway migrations |

## Getting Started

### Prerequisites

- Java 17+
- Node.js 18+
- Docker & Docker Compose
- An OpenAI API key ([get one here](https://platform.openai.com/api-keys))

### 1. Clone and configure

```bash
git clone https://github.com/daoninhthai/ai-chatbot-spring.git
cd ai-chatbot-spring
cp .env.example .env
```

Edit `.env` and add your OpenAI API key:
```
OPENAI_API_KEY=sk-your-key-here
```

### 2. Start the database

```bash
docker-compose -f docker-compose.dev.yml up -d
```

### 3. Run the backend

```bash
./mvnw spring-boot:run
```

The API will be available at `http://localhost:8080`.

### 4. Run the frontend

```bash
cd frontend
npm install
npm run dev
```

Open `http://localhost:3000` in your browser.

### Alternative: Run everything with Docker

```bash
docker-compose up --build
```

## API Endpoints

Once the app is running, check out the full API docs at:
**http://localhost:8080/swagger-ui.html**

Quick overview:

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/api/auth/register` | POST | Create account |
| `/api/auth/login` | POST | Get JWT tokens |
| `/api/auth/refresh` | POST | Refresh access token |
| `/api/auth/me` | GET | Current user info |
| `/api/conversations` | GET/POST | List/create conversations |
| `/api/conversations/{id}` | GET/PUT/DELETE | Manage a conversation |
| `/api/chat/{id}` | POST | Send message, get response |
| `/api/chat/{id}/stream` | GET (SSE) | Stream response in real-time |
| `/api/chat/{id}/messages` | GET | Message history |
| `/api/documents/upload` | POST | Upload document for RAG |
| `/api/documents` | GET | List documents |
| `/api/documents/{id}` | GET/DELETE | Manage a document |

## Project Structure

```
ai-chatbot-spring/
├── src/main/java/com/daoninhthai/aichatbot/
│   ├── config/          # Spring AI, Security, Rate limiting, CORS, OpenAPI
│   ├── controller/      # REST endpoints (Auth, Chat, Conversations, Documents)
│   ├── dto/             # Request/response objects
│   ├── entity/          # JPA entities (User, Conversation, Message, Document)
│   ├── repository/      # Data access
│   ├── service/         # Business logic (ChatService with streaming, DocumentService with RAG)
│   ├── security/        # JWT token provider, custom UserDetails
│   └── exception/       # Global error handling
├── frontend/
│   └── src/
│       ├── api/         # Axios client with JWT interceptor
│       ├── components/  # Chat UI, auth forms, document upload, layout
│       ├── context/     # Auth and theme state
│       ├── pages/       # Route-level components
│       └── types/       # TypeScript interfaces
├── docker-compose.yml        # Production stack
├── docker-compose.dev.yml    # Dev database
└── Dockerfile                # Multi-stage build
```

## What's Next (v2.0 Ideas)

- [ ] WebSocket support for multi-user real-time chat
- [ ] Admin dashboard with usage analytics
- [ ] Multiple AI model support (Claude, Gemini)
- [ ] File preview in the knowledge base
- [ ] Chat export (PDF, Markdown)
- [ ] Token usage tracking and billing

## Author

Java Full Stack Developer.

If you're looking for someone to build AI-powered applications, chatbots, or enterprise backends with Spring Boot, feel free to reach out.
