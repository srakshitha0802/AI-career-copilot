# AI Career Copilot

An enterprise-grade AI platform that helps job seekers: optimize resumes, generate ATS-scored feedback, semantically match jobs, and track application progress.

> **Monorepo**: `backend/` (Spring Boot) + `frontend/` (React + Vite) + optional `chrome-extension/`.

---

## Features

- **Resume parsing & ATS scoring** (PDF/DOCX via Apache Tika)
- **Semantic job matching** using vector search (Postgres + pgvector)
- **AI-assisted outputs** (job matching feedback, cover letter, interview prep templates)
- **Authentication & security** (JWT + Google Authenticator TOTP 2FA)
- **Multi-service runtime**: Redis caching/rate limits, RabbitMQ async workers, email notifications
- **Storage abstraction** (local uploads; designed to plug in S3/Azure)

---

## Tech Stack

### Backend
- Java/Spring Boot (modular domain structure / DDD-style)
- PostgreSQL + **pgvector**
- Redis (cache + rate limiting)
- RabbitMQ (async processing)
- Flyway migrations

### Frontend
- React + TypeScript
- Vite
- TailwindCSS
- Axios

---

## Repo Layout

```text
AI career copilet/
├── backend/            # Spring Boot API + DB migrations
├── frontend/           # React SPA
├── chrome-extension/  # Optional extension
├── docker-compose.yml # Local dev stack (Postgres/Redis/RabbitMQ/Backend/Frontend)
└── README.md
```

---

## Local Development (Recommended: Docker Compose)

This uses the provided `docker-compose.yml` to start the full dependency stack.

### 1) Create/Export environment variables
Copy the example env file (if present):

```bash
cp .env.example .env
```

If `.env.example` doesn’t exist in your environment, you can still run compose since most values have defaults in `docker-compose.yml`.

### 2) Start services
```bash
docker compose up --build
```

**Endpoints (default):**
- Backend: `http://localhost:8080`
- Frontend: `http://localhost:3000`
- RabbitMQ UI: `http://localhost:15672`
- MailHog UI: `http://localhost:8025`

---

## Local Development (Without Docker)

### Backend
```bash
cd backend
./mvnw spring-boot:run
```

### Frontend
```bash
cd frontend
npm install
npm run dev
```

---

## Common Developer Workflows

### Run DB migrations
Flyway runs automatically on backend startup.

### Upload/Process resumes
- Upload a resume from the frontend
- Backend parses it and enqueues async work to RabbitMQ

---

## Notes for Deployment

- Set `JWT_SECRET` to a long, random value
- Provide AI provider keys (`OPENAI_API_KEY` / `GEMINI_API_KEY`) and set `AI_DEFAULT_PROVIDER`
- Configure storage provider (`STORAGE_PROVIDER`) and credentials for production

---

## License

Add your project license here (e.g., MIT).

