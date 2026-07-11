# AI Career Copilot

An enterprise-grade, production-ready AI-powered career platform designed to help job seekers optimize their resumes, analyze ATS compatibility, match roles semantically, and track applications.

---

## Technical Stack & Architecture

### Backend: Modular Monolith (Spring Boot 3.5.x + Java 21 LTS)
Organized using Domain-Driven Design (DDD) boundaries and Event-Driven Architecture (EDA):
* **Identity**: Rotary JWT security, TOTP multi-factor authentication (MFA/2FA), and Multi-tenancy.
* **Resume**: Version history storage, Apache Tika parsing, and Deterministic ATS score engine.
* **Job**: Provider integrations (Greenhouse/Lever) and `@Scheduled` cron crawlers.
* **Match**: Vector semantic search matching calculations.
* **Application**: Track application stages on a Kanban board.
* **AI**: Template resolution prompts and cost logger.
* **Storage**: Local filesystem mock object adapter, ready for S3 configuration.

### Services & Stores
* **Database**: PostgreSQL 18 + `pgvector` extension (storing 1536-dimensional embeddings).
* **Caching**: Redis (Session caches & API rate limits).
* **Broker**: RabbitMQ (Asynchronous worker enqueuing).

### Frontend: React + TypeScript + Tailwind CSS
Vite-powered Single Page Application (SPA) utilizing Lucide Icons, Recharts visual statistics, and custom glassmorphic themes.

---

## Directory Structure

```
AI career copilet/
├── backend/            # Spring Boot Modular Monolith
│   ├── src/main/java/  # Java 21 Domain Modules
│   └── pom.xml         # Maven project descriptor
├── frontend/           # React TypeScript Frontend
│   └── src/            # Components, contexts, and pages
├── README.md           # Setup instructions
└── task.md             # Verification checklist
```

---

## Quick Start & Running Locally

Ensure PostgreSQL, Redis, and RabbitMQ are running on your host machine (e.g., via Homebrew on macOS):
```bash
brew services start postgresql@18
brew services start redis
brew services start rabbitmq
```

### 1. Database Initialization
Ensure a database named `careercopilot` is created:
```bash
psql -U $(whoami) -d postgres -c "CREATE DATABASE careercopilot;"
```
*(Flyway will automatically execute migrations and create schemas upon backend boot).*

### 2. Running the Backend
From the `backend/` directory:
```bash
./mvnw spring-boot:run
```
The server will initialize on `http://localhost:8080`.

### 3. Running the Frontend
From the `frontend/` directory:
```bash
npm install
npm run dev
```
The application will launch on `http://localhost:5173`.

---

## Verification & Key Scenarios

### Authentication & 2FA Flow
1. Sign up a new user via the React interface `/signup` or REST calls.
2. Log in and navigate to the **MFA Security** tab on your dashboard.
3. Click **Setup Google 2FA**, scan the QR code with your mobile authenticator app, and type the 6-digit verification code to confirm activation.
4. Next time you log in, the system will challenge you with a 2FA prompt before granting JWT credentials!

### Semantic Job Match Checklist
1. Navigate to the **Resume Analyzer** tab and upload a test CV (PDF/Docx format).
2. The file is uploaded, parsed via Apache Tika, and enqueued in RabbitMQ.
3. The RabbitMQ background worker calls the AI client to construct structured fields, calculate pgvector semantic embedding points, and generate deterministic ATS reports.
4. Navigate to the **Job Matching** tab. The system pulls and ranks jobs from Greenhouse/Lever using Cosine Distance (`<=>`) queries!
5. Select a job card to view custom AI alignment feedback (Strengths, Gaps, and recommendations).
# AI-career-copilot
# AI-career-copilot
