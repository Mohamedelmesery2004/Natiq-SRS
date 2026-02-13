# Natiq - Software Requirements Specification (SRS)

| Field | Value |
|-------|-------|
| **Document Title** | Software Requirements Specification (SRS) |
| **Project Name** | Natiq - AI-Powered Omnichannel Contact Center SaaS |
| **Version** | 1.0 |
| **Date** | February 2026 |
| **Status** | In Progress |
| **Confidentiality** | Internal Use Only |

---

## Table of Contents

1. [Introduction](#1-introduction)
2. [System Architecture Overview](#2-system-architecture-overview)
3. [Technology Stack](#3-technology-stack)
4. [Data Model Summary](#4-data-model-summary)
5. [Authentication & Authorization Specification](#5-authentication--authorization-specification)
6. [API Design Standards](#6-api-design-standards)
7. [Cycle 1 - Foundation & Multi-Tenant Core](#cycle-1---foundation--multi-tenant-core-1)
8. [Cycle 2 - AI Chat Engine & Knowledge Base](#cycle-2---ai-chat-engine--knowledge-base-1)
9. [Cycle 3 - Omnichannel Integration](#cycle-3---omnichannel-integration-1)
10. [Cycle 4 - Ticket Management System](#cycle-4---ticket-management-system-1)
11. [Cycle 5 - Agent Workspace](#cycle-5---agent-workspace-1)
12. [Cycle 6 - SaaS Billing & Subscription Management](#cycle-6---saas-billing--subscription-management-1)
13. [Cycle 7 - Department & Team Management](#cycle-7---department--team-management-1)
14. [Cycle 8 - Performance Analytics & Quality Assurance](#cycle-8---performance-analytics--quality-assurance-1)
15. [Cycle 9 - Sentiment Analysis & AI Intelligence](#cycle-9---sentiment-analysis--ai-intelligence-1)
16. [Cycle 10 - Notifications, Tags & Operational Tools](#cycle-10---notifications-tags--operational-tools-1)
17. [Cycle 11 - Admin Dashboard & Reporting](#cycle-11---admin-dashboard--reporting-1)
18. [Cycle 12 - Testing, Security & Production Launch](#cycle-12---testing-security--production-launch-1)
19. [Non-Functional Requirements](#non-functional-requirements)
20. [External Interfaces](#external-interfaces)

---

## 1. Introduction

### 1.1 Purpose

This Software Requirements Specification (SRS) document provides the complete technical specification for the Natiq platform. It describes the functional requirements (what the system does), non-functional requirements (how well it does it), data models, API contracts, and technical constraints for each development cycle.

### 1.2 Scope

Natiq is a backend SaaS platform (REST API + WebSocket) built with Node.js. This SRS covers:
- All 12 development cycles
- 21 database entities with full schema definitions
- REST API endpoints with request/response contracts
- Socket.IO event specifications
- Third-party integrations (Telegram Bot API, Groq LLM API)
- Security, performance, and scalability requirements

### 1.3 Definitions & Conventions

- **FR**: Functional Requirement
- **NFR**: Non-Functional Requirement
- **Endpoint**: REST API route in format `METHOD /path`
- **Socket Event**: Socket.IO event in format `event_name`
- **Schema**: MongoDB/Mongoose document structure
- All IDs are MongoDB ObjectId (24-character hex strings)
- All timestamps are ISO 8601 format in UTC
- All endpoints return JSON with structure: `{ success: boolean, message: string, data: object }`

---

## 2. System Architecture Overview

```
┌─────────────────────────────────────────────────────────────┐
│                       CLIENTS                                │
│  ┌──────────┐  ┌──────────────┐  ┌────────────────────────┐ │
│  │ Telegram  │  │ Web Browser  │  │ Admin Dashboard (SPA)  │ │
│  │ Bot Users │  │ Chat Widget  │  │ Agent Workspace (SPA)  │ │
│  └─────┬─────┘  └──────┬───────┘  └───────────┬────────────┘ │
└────────┼───────────────┼──────────────────────┼──────────────┘
         │               │                      │
         │ HTTPS         │ WSS (Socket.IO)      │ HTTPS + WSS
         ▼               ▼                      ▼
┌─────────────────────────────────────────────────────────────┐
│                    NATIQ BACKEND (Node.js)                    │
│                                                               │
│  ┌──────────────────────────────────────────────────────┐    │
│  │                   Express.js Server                    │    │
│  │                                                        │    │
│  │  ┌─────────────┐  ┌─────────────┐  ┌──────────────┐  │    │
│  │  │   REST API   │  │  Socket.IO  │  │  Middlewares  │  │    │
│  │  │   Routes     │  │  /admin NS  │  │  - protect    │  │    │
│  │  │  /api/v1/*   │  │  /webchat NS│  │  - tenant     │  │    │
│  │  └──────┬───────┘  └──────┬──────┘  │  - rbac       │  │    │
│  │         │                 │         │  - validate    │  │    │
│  │         ▼                 ▼         └──────────────┘  │    │
│  │  ┌──────────────────────────────┐                      │    │
│  │  │       Controllers            │                      │    │
│  │  │  admin, agent, channel       │                      │    │
│  │  └──────────┬───────────────────┘                      │    │
│  │             ▼                                          │    │
│  │  ┌──────────────────────────────┐                      │    │
│  │  │        Services              │                      │    │
│  │  │  chat, agent, analytics      │                      │    │
│  │  └──────────┬───────────────────┘                      │    │
│  │             ▼                                          │    │
│  │  ┌──────────────────────────────┐                      │    │
│  │  │       Models (Mongoose)      │                      │    │
│  │  │  21 entities                 │                      │    │
│  │  └──────────┬───────────────────┘                      │    │
│  └─────────────┼────────────────────────────────────────┘    │
│                ▼                                              │
│  ┌──────────────────┐  ┌──────────────┐  ┌──────────────┐   │
│  │    MongoDB        │  │  File System │  │ Cron Jobs    │   │
│  │    (Primary DB)   │  │  /uploads/   │  │ (node-cron)  │   │
│  └──────────────────┘  └──────────────┘  └──────────────┘   │
└─────────────────────────────────────────────────────────────┘
         │                                       │
         ▼                                       ▼
┌──────────────────┐                  ┌──────────────────┐
│  Groq LLM API    │                  │ Telegram Bot API  │
│  (AI Responses)  │                  │ (Channel Delivery) │
└──────────────────┘                  └──────────────────┘
```

### 2.1 Key Architecture Decisions

| Decision | Rationale |
|----------|-----------|
| **Multi-tenant with shared database** | All companies share one MongoDB instance with companyId isolation. Simpler than database-per-tenant, sufficient for initial scale (<100 companies) |
| **Embedded subdocuments for Messages and AgentNotes** | Messages are always loaded with their parent (ChatSession/Ticket). Embedding avoids JOINs and maintains ordering. Tradeoff: 16MB document limit (~100K messages per session — sufficient) |
| **Socket.IO namespaces for isolation** | /admin namespace for staff, /webchat namespace for customers. Prevents customers from receiving admin events |
| **JWT for stateless authentication** | No server-side session storage. Tokens carry companyId and role for every request. 7-day expiry balances security and convenience |
| **Service layer pattern** | Controllers handle HTTP concerns; services handle business logic. Services are reusable across REST and Socket handlers |

---

## 3. Technology Stack

| Layer | Technology | Version | Purpose |
|-------|-----------|---------|---------|
| Runtime | Node.js | 18+ LTS | Server-side JavaScript execution |
| Framework | Express.js | 4.x | HTTP server and routing |
| Real-time | Socket.IO | 4.x | WebSocket communication for live updates |
| Database | MongoDB | 6.x+ | Primary data store |
| ODM | Mongoose | 7.x+ | Schema validation, querying, indexing |
| Authentication | jsonwebtoken | 9.x | JWT generation and verification |
| Password Hashing | bcrypt | 5.x | Secure password storage (12 rounds) |
| Validation | Joi | 17.x | Request body/param/query validation |
| File Upload | Multer | 1.x | Multipart form-data handling |
| AI Provider | Groq SDK | latest | LLM API for AI responses |
| HTTP Client | Axios | 1.x | External API calls (Telegram, Groq) |
| Cron | node-cron | 3.x | Scheduled tasks (daily performance snapshots) |
| Security | Helmet | 7.x | HTTP security headers |
| CORS | cors | 2.x | Cross-origin resource sharing |
| Rate Limiting | express-rate-limit | 7.x | API abuse prevention |

---

## 4. Data Model Summary

Full entity details are documented in `ERD-Documentation.md`. Summary:

| # | Entity | Collection Name | Multi-tenant | Relationship |
|---|--------|----------------|-------------|--------------|
| 1 | Company | companies | Root entity | 1:N to all entities |
| 2 | User | users | Yes (companyId) | N:1 Company, N:1 Department |
| 3 | ChatSession | chatsessions | Yes (companyId) | N:1 User, 1:N Message (embedded) |
| 4 | Ticket | tickets | Yes (companyId) | N:1 User, N:1 User (assignedTo) |
| 5 | KnowledgeItem | knowledgeitems | Yes (companyId) | N:1 Company |
| 6 | EventLog | eventlogs | Yes (companyId) | Polymorphic (entityType + entityId) |
| 7 | Plan | plans | No (platform-wide) | 1:N Subscription |
| 8 | Subscription | subscriptions | Yes (companyId) | N:1 Plan, N:1 Company |
| 9 | Invoice | invoices | Yes (companyId) | N:1 Subscription, N:1 Company |
| 10 | Department | departments | Yes (companyId) | N:1 Company, 1:N User |
| 11 | AgentPerformance | agentperformances | Yes (companyId) | N:1 User |
| 12 | Rating | ratings | Yes (companyId) | N:1 Ticket, N:1 User |
| 13 | QAReview | qareviews | Yes (companyId) | N:1 ChatSession, N:1 User |
| 14 | CannedResponse | cannedresponses | Yes (companyId) | N:1 Company |
| 15 | Tag | tags | Yes (companyId) | N:N Ticket (via TicketTag) |
| 16 | TicketTag | tickettags | Implicit | N:1 Ticket, N:1 Tag |
| 17 | Notification | notifications | Yes (companyId) | N:1 User |
| 18 | Attachment | attachments | Yes (companyId) | Polymorphic (entityType + entityId) |
| 19 | SentimentLog | sentimentlogs | Yes (companyId) | N:1 ChatSession |
| 20 | AIConfig | aiconfigs | Yes (companyId) | 1:1 Company |

---

## 5. Authentication & Authorization Specification

### 5.1 Authentication Flow

```
Client                          Server
  │                                │
  │  POST /auth/login              │
  │  {email, password, companySlug}│
  │  ─────────────────────────────>│
  │                                │ 1. Find company by slug
  │                                │ 2. Find user by email + companyId
  │                                │ 3. bcrypt.compare(password, passwordHash)
  │                                │ 4. Generate JWT: {userId, companyId, role}
  │  {token, user}                 │
  │  <─────────────────────────────│
  │                                │
  │  GET /api/v1/tickets           │
  │  Authorization: Bearer <token> │
  │  ─────────────────────────────>│
  │                                │ 1. protect middleware: verify JWT
  │                                │ 2. tenantIsolation: set req.companyId
  │                                │ 3. requirePermission: check RBAC matrix
  │                                │ 4. Controller handles request
  │  {success, data}               │
  │  <─────────────────────────────│
```

### 5.2 JWT Token Payload

```json
{
  "userId": "ObjectId",
  "companyId": "ObjectId",
  "role": "string (enum: ROLES)",
  "iat": "number (issued at)",
  "exp": "number (expires in 7 days)"
}
```

### 5.3 Middleware Chain

Every protected route passes through this chain in order:

| # | Middleware | Purpose | Failure Response |
|---|-----------|---------|-----------------|
| 1 | `protect` | Verify JWT token, load user from DB, set req.userId, req.companyId, req.user | 401 Unauthorized |
| 2 | `tenantIsolation` | Ensure req.companyId is set. For super_admin with body.companyId, use that instead | 400 Bad Request |
| 3 | `requirePermission(resource, action)` or `allowRoles(...roles)` | Check RBAC_MATRIX[role][resource].includes(action) | 403 Forbidden |
| 4 | `validate(schema)` | Joi validation of req.body, req.params, req.query. stripUnknown: true, abortEarly: false | 400 Bad Request with details |
| 5 | Controller | Business logic execution | Various |

### 5.4 RBAC Matrix

```
                  companies  users  knowledge  chat  tickets  embeddings  analytics
super_admin       CRUDM      CRUDM  CRUDM      RDM   CRUDM    CRM         RM
company_manager   -          CRUD   CRUD       RUD   RUM      CR          R
team_leader       -          CRU    CRU        RU    RU       R           R
agent             -          -      R          RU    RU       -           R
customer          -          -      -          CR    R        -           -

C=Create R=Read U=Update D=Delete M=Manage
```

---

## 6. API Design Standards

### 6.1 URL Structure

```
Base URL: /api/v1

Admin Routes:     /api/v1/admin/*        (company_manager, team_leader, super_admin)
Agent Routes:     /api/v1/agent/*        (agent role only)
Channel Routes:   /api/v1/channels/*     (public webhooks, customer-facing)
```

### 6.2 Response Format

**Success Response:**
```json
{
  "success": true,
  "message": "Descriptive success message",
  "data": { ... }
}
```

**Error Response:**
```json
{
  "success": false,
  "message": "Descriptive error message",
  "errors": [ ... ]   // Optional: validation errors array
}
```

### 6.3 HTTP Status Codes

| Code | Usage |
|------|-------|
| 200 | Successful GET, PATCH, DELETE |
| 201 | Successful POST (resource created) |
| 400 | Validation error, bad request (ApiError.badRequest) |
| 401 | Missing/invalid JWT token (ApiError.unauthorized) |
| 403 | Valid token but insufficient permissions (ApiError.forbidden) |
| 404 | Resource not found (ApiError.notFound) |
| 409 | Conflict - duplicate resource (ApiError.conflict) |
| 500 | Internal server error (ApiError.internal) |

### 6.4 Pagination Standard

All list endpoints support:

```
GET /api/v1/admin/tickets?page=1&limit=20&sort=-createdAt

Response includes:
{
  "data": {
    "items": [...],
    "pagination": {
      "page": 1,
      "limit": 20,
      "total": 150,
      "pages": 8
    }
  }
}
```

### 6.5 Validation Standard

All request validation uses Joi with:
```javascript
{
  abortEarly: false,     // Report all errors, not just the first
  stripUnknown: true,    // Remove fields not in the schema
  allowUnknown: false    // Reject unknown fields
}
```

---

## Cycle 1 - Foundation & Multi-Tenant Core

### Prerequisites
- Node.js 18+ installed
- MongoDB 6.x+ running
- No prior system dependencies

### Entities Created

**Company** — See ERD-Documentation.md Section 4

**User** — See ERD-Documentation.md Section 6

### API Endpoints

#### Company Management

| Method | Endpoint | Auth | Roles | Description |
|--------|----------|------|-------|-------------|
| POST | `/api/v1/admin/companies` | JWT | super_admin | Create a new company |
| GET | `/api/v1/admin/companies` | JWT | super_admin | List all companies (paginated) |
| GET | `/api/v1/admin/companies/:id` | JWT | super_admin | Get company by ID |
| PATCH | `/api/v1/admin/companies/:id` | JWT | super_admin | Update company settings |
| DELETE | `/api/v1/admin/companies/:id` | JWT | super_admin | Deactivate company (soft delete) |

**POST /api/v1/admin/companies — Request:**
```json
{
  "name": "Vodafone Egypt",
  "slug": "vodafone-egypt",
  "industry": "telecom",
  "channelsConfig": {
    "telegram": { "botToken": "123:ABC", "isActive": true },
    "webChat": { "isActive": true }
  },
  "settings": {
    "aiEnabled": true,
    "escalationThreshold": 0.5,
    "maxSessionMessages": 50,
    "workingHours": { "start": "09:00", "end": "17:00", "timezone": "Africa/Cairo" }
  }
}
```

**Response (201):**
```json
{
  "success": true,
  "message": "Company created",
  "data": {
    "company": {
      "_id": "ObjectId",
      "name": "Vodafone Egypt",
      "slug": "vodafone-egypt",
      "industry": "telecom",
      "channelsConfig": { ... },
      "settings": { ... },
      "isActive": true,
      "createdAt": "2026-02-01T00:00:00.000Z",
      "updatedAt": "2026-02-01T00:00:00.000Z"
    }
  }
}
```

#### User Management

| Method | Endpoint | Auth | Roles | Description |
|--------|----------|------|-------|-------------|
| POST | `/api/v1/admin/users` | JWT | super_admin, company_manager, team_leader | Create a new user |
| GET | `/api/v1/admin/users` | JWT | super_admin, company_manager, team_leader | List users (paginated, filtered) |
| GET | `/api/v1/admin/users/:id` | JWT | super_admin, company_manager, team_leader | Get user by ID |
| PATCH | `/api/v1/admin/users/:id` | JWT | super_admin, company_manager | Update user |
| DELETE | `/api/v1/admin/users/:id` | JWT | super_admin, company_manager | Deactivate user (soft delete) |

#### Authentication

| Method | Endpoint | Auth | Roles | Description |
|--------|----------|------|-------|-------------|
| POST | `/api/v1/admin/auth/login` | None | super_admin, company_manager, team_leader | Staff login |
| GET | `/api/v1/admin/auth/me` | JWT | Any staff | Get current user profile |

**POST /api/v1/admin/auth/login — Request:**
```json
{
  "email": "admin@vodafone.com",
  "password": "securePassword123",
  "companySlug": "vodafone-egypt"
}
```

**Response (200):**
```json
{
  "success": true,
  "message": "Login successful",
  "data": {
    "token": "eyJhbGciOiJIUzI1NiIs...",
    "user": {
      "_id": "ObjectId",
      "name": "Admin User",
      "email": "admin@vodafone.com",
      "role": "company_manager",
      "companyId": "ObjectId",
      "isActive": true
    }
  }
}
```

### Validation Rules

| Field | Rules |
|-------|-------|
| `name` | Required, string, 2-100 chars, trimmed |
| `email` | Required, valid email format, lowercase, trimmed |
| `password` | Required on create, min 6 chars |
| `role` | Required, must be valid ROLES enum value |
| `companySlug` | Required on login, lowercase, trimmed |
| `slug` | Required on company create, lowercase, alphanumeric + hyphens, unique |
| `industry` | Optional, must be valid enum value |

---

## Cycle 2 - AI Chat Engine & Knowledge Base

### Prerequisites
- Cycle 1 complete (Company, User, Auth)
- Groq API key configured in environment
- Embedding model API access

### Entities Created

**ChatSession** — See ERD-Documentation.md Section 7

**Message** (embedded) — See ERD-Documentation.md Section 8

**KnowledgeItem** — See ERD-Documentation.md Section 14

### API Endpoints

#### Knowledge Base

| Method | Endpoint | Auth | Roles | Description |
|--------|----------|------|-------|-------------|
| POST | `/api/v1/admin/knowledge` | JWT | company_manager, team_leader | Create knowledge item |
| GET | `/api/v1/admin/knowledge` | JWT | Any staff | List knowledge items (paginated) |
| GET | `/api/v1/admin/knowledge/:id` | JWT | Any staff | Get knowledge item by ID |
| PATCH | `/api/v1/admin/knowledge/:id` | JWT | company_manager, team_leader | Update knowledge item |
| DELETE | `/api/v1/admin/knowledge/:id` | JWT | company_manager | Delete knowledge item |

#### Embeddings

| Method | Endpoint | Auth | Roles | Description |
|--------|----------|------|-------|-------------|
| POST | `/api/v1/admin/embeddings/generate` | JWT | company_manager | Generate embeddings for all active knowledge items |
| GET | `/api/v1/admin/embeddings/status` | JWT | company_manager | Check embedding status |

#### Chat Sessions (Admin View)

| Method | Endpoint | Auth | Roles | Description |
|--------|----------|------|-------|-------------|
| GET | `/api/v1/admin/chat/sessions` | JWT | company_manager, team_leader | List sessions (paginated, filtered) |
| GET | `/api/v1/admin/chat/sessions/:id` | JWT | company_manager, team_leader | Get session with messages |
| DELETE | `/api/v1/admin/chat/sessions/:id` | JWT | company_manager | Close/delete session |

### AI Pipeline Specification

```
Customer Message Received
         │
         ▼
┌─────────────────────┐
│ 1. Load ChatSession  │  Find active session or create new one
│    (or create new)   │
└─────────┬───────────┘
          │
          ▼
┌─────────────────────┐
│ 2. Check if agent   │  if (session.isAgentHandling && !idle>5min)
│    is handling       │  → Forward to agent, skip AI
└─────────┬───────────┘
          │ (AI handling)
          ▼
┌─────────────────────┐
│ 3. Check escalation │  Match keywords: agent, human, ticket, complain
│    keywords          │  → Create ticket if matched
└─────────┬───────────┘
          │ (no escalation)
          ▼
┌─────────────────────┐
│ 4. RAG: Find        │  Compute message embedding
│    relevant KB items │  Cosine similarity search on KnowledgeItem.embeddingVector
│                      │  Top 3 items as context
└─────────┬───────────┘
          │
          ▼
┌─────────────────────┐
│ 5. LLM Call (Groq)  │  System prompt + KB context + message history + user message
│                      │  Model: llama-3.3-70b-versatile
│                      │  Temperature: 0.4, maxTokens: 512
└─────────┬───────────┘
          │
          ▼
┌─────────────────────┐
│ 6. Check AI response│  If shouldEscalate flag or low confidence
│    for escalation   │  → Create ticket
└─────────┬───────────┘
          │
          ▼
┌─────────────────────┐
│ 7. Save & Respond   │  Save message + response to ChatSession
│                      │  Send response via channel (Telegram/Web)
└─────────────────────┘
```

### LLM Request Structure

```json
{
  "model": "llama-3.3-70b-versatile",
  "messages": [
    {
      "role": "system",
      "content": "You are a customer service assistant for {companyName}. Use ONLY the following knowledge base to answer...\n\nKnowledge Base:\n{relevantKBItems}\n\nRules:\n- Respond in the customer's language\n- If you cannot answer, set shouldEscalate: true\n- Never hallucinate ticket creation\n- Respond in JSON: {response, shouldEscalate, detectedIntent, confidence}"
    },
    { "role": "user", "content": "Previous message 1" },
    { "role": "assistant", "content": "Previous response 1" },
    { "role": "user", "content": "Current message" }
  ],
  "temperature": 0.4,
  "max_tokens": 512
}
```

---

## Cycle 3 - Omnichannel Integration

### Prerequisites
- Cycle 2 complete (ChatSession, AI Pipeline)
- Telegram bot created via @BotFather
- Company.channelsConfig.telegram.botToken set

### Entities Modified

**User** — Added: `telegramChatId`, `onboardingStep`

### API Endpoints

#### Telegram Channel

| Method | Endpoint | Auth | Roles | Description |
|--------|----------|------|-------|-------------|
| POST | `/api/v1/channels/telegram/:companySlug/webhook` | Webhook (no JWT) | Public | Receive Telegram updates |

#### Webhook Processing Flow

```
Telegram Server
      │
      │ POST /api/v1/channels/telegram/vodafone-egypt/webhook
      │ Body: { update_id, message: { chat: { id }, text, from } }
      ▼
┌───────────────────────┐
│ 1. Find Company       │  By slug from URL params
│    by slug            │  Validate channelsConfig.telegram.isActive
└──────────┬────────────┘
           │
           ▼
┌───────────────────────┐
│ 2. Find/Create User   │  By telegramChatId = message.chat.id
│    by telegramChatId  │  If new: create with role=customer, onboardingStep=1
└──────────┬────────────┘
           │
           ▼
┌───────────────────────┐
│ 3. Handle Commands    │  /start → close old session + welcome
│                       │  /reset → close session + confirm
│                       │  /status → show ticket status
└──────────┬────────────┘
           │ (regular message)
           ▼
┌───────────────────────┐
│ 4. Check Onboarding   │  step=1 → save name, ask phone
│                       │  step=2 → save phone, step=0
│                       │  step=0 → proceed to AI pipeline
└──────────┬────────────┘
           │
           ▼
┌───────────────────────┐
│ 5. AI Pipeline        │  Same as Cycle 2 specification
│    (chat.service)     │  Response sent via Telegram Bot API
└───────────────────────┘
```

#### Telegram Bot API Calls

```
Send Message:
POST https://api.telegram.org/bot{token}/sendMessage
{
  "chat_id": "{telegramChatId}",
  "text": "{responseText}",
  "parse_mode": "Markdown"
}

On parse failure → retry without parse_mode
```

### Socket.IO Specification — Web Chat

**Namespace:** `/webchat`

**Connection:** `io("wss://domain/webchat", { auth: { sessionId, companyId } })`

| Event | Direction | Payload | Description |
|-------|-----------|---------|-------------|
| `chat:sendMessage` | Client → Server | `{ sessionId, content }` | Customer sends a message |
| `chat:message` | Server → Client | `{ sessionId, message: { role, content, timestamp } }` | AI or agent response |
| `chat:sessionCreated` | Server → Client | `{ sessionId }` | New session started |
| `chat:sessionClosed` | Server → Client | `{ sessionId }` | Session ended |
| `chat:typing` | Server → Client | `{ sessionId }` | AI is processing |

---

## Cycle 4 - Ticket Management System

### Prerequisites
- Cycle 2 complete (ChatSession for context linking)
- Cycle 3 complete (channel info for response delivery)

### Entities Created

**Ticket** — See ERD-Documentation.md Section 10

**EventLog** — See ERD-Documentation.md Section 20

### API Endpoints

#### Ticket Management (Admin)

| Method | Endpoint | Auth | Roles | Description |
|--------|----------|------|-------|-------------|
| GET | `/api/v1/admin/tickets` | JWT | company_manager, team_leader | List all tickets (paginated, filtered) |
| GET | `/api/v1/admin/tickets/:id` | JWT | company_manager, team_leader | Get ticket details |
| PATCH | `/api/v1/admin/tickets/:id` | JWT | company_manager, team_leader | Update ticket (status, priority, category, assignedTo) |

**GET /api/v1/admin/tickets — Query Parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `page` | Number | Page number (default: 1) |
| `limit` | Number | Items per page (default: 20, max: 100) |
| `status` | String | Filter by status (open, in_progress, resolved, closed) |
| `category` | String | Filter by category |
| `priority` | String | Filter by priority |
| `assignedTo` | ObjectId | Filter by assigned agent |
| `channel` | String | Filter by originating channel |
| `sort` | String | Sort field with - prefix for desc (default: -createdAt) |
| `from` | Date | Created after this date |
| `to` | Date | Created before this date |

### Ticket Number Generation

```
Format: NQ-YYYYMMDD-NNNN

NQ         = Natiq prefix
YYYYMMDD   = Date of creation
NNNN       = Sequential number per company per day (zero-padded)

Example: NQ-20260209-0001 (first ticket for this company on Feb 9, 2026)

Generation: Count existing tickets for (companyId, today) + 1
```

### Ticket State Machine

```
                ┌──────────────────┐
                │      OPEN        │  Created by AI escalation or manually
                │  (assignedTo:    │
                │   null)          │
                └────────┬─────────┘
                         │
                         │ Agent claims (POST /claim)
                         ▼
                ┌──────────────────┐
                │   IN_PROGRESS    │  Agent working on it
                │  (assignedTo:    │
                │   agentId)       │  ←── Can reassign to another agent
                └────────┬─────────┘
                         │
                         │ Agent resolves (PATCH status=resolved)
                         ▼
                ┌──────────────────┐
                │    RESOLVED      │  Issue addressed, awaiting confirmation
                │  (resolvedAt     │
                │   set)           │  ←── Can reopen to IN_PROGRESS
                └────────┬─────────┘
                         │
                         │ Confirmed done (PATCH status=closed)
                         ▼
                ┌──────────────────┐
                │     CLOSED       │  Archived, metrics finalized
                │  (terminal)      │
                └──────────────────┘
```

### Event Logging

Every ticket lifecycle event creates an EventLog entry:

| Event | eventType | metadata |
|-------|-----------|----------|
| Ticket created by AI | `ticket_created` | `{ channel, category, intent, confidence, sessionId }` |
| Ticket claimed by agent | `ticket_claimed` | `{ agentId, agentName }` |
| Agent replied | `agent_replied` | `{ agentId, channel }` |
| Ticket resolved | `ticket_resolved` | `{ agentId, resolutionTimeMs }` |
| Ticket closed | `ticket_closed` | `{ closedBy }` |
| Ticket updated | `ticket_updated` | `{ changedFields, updatedBy }` |

---

## Cycle 5 - Agent Workspace

### Prerequisites
- Cycle 3 complete (Socket.IO infrastructure)
- Cycle 4 complete (Ticket system)

### API Endpoints

#### Agent Authentication

| Method | Endpoint | Auth | Roles | Description |
|--------|----------|------|-------|-------------|
| POST | `/api/v1/agent/auth/login` | None | agent | Agent-specific login |

#### Agent Profile

| Method | Endpoint | Auth | Roles | Description |
|--------|----------|------|-------|-------------|
| GET | `/api/v1/agent/profile` | JWT | agent | Get own profile |
| PATCH | `/api/v1/agent/profile` | JWT | agent | Update profile (multipart/form-data) |

**PATCH /api/v1/agent/profile — Multipart Form-Data:**

| Field | Type | Description |
|-------|------|-------------|
| `name` | text | Optional, 2-100 chars |
| `phone` | text | Optional |
| `password` | text | Optional, min 6 chars (requires currentPassword) |
| `currentPassword` | text | Required if changing password |
| `profileImage` | file | Optional, jpeg/jpg/png/gif/webp, max 5MB |

#### Agent Dashboard

| Method | Endpoint | Auth | Roles | Description |
|--------|----------|------|-------|-------------|
| GET | `/api/v1/agent/dashboard` | JWT | agent | Personal stats overview |

**Response:**
```json
{
  "data": {
    "assignedTickets": 5,
    "resolvedToday": 3,
    "avgResponseTimeMs": 45000,
    "openTicketsInQueue": 12
  }
}
```

#### Agent Ticket Operations

| Method | Endpoint | Auth | Roles | Description |
|--------|----------|------|-------|-------------|
| GET | `/api/v1/agent/tickets` | JWT | agent | List tickets (my assigned + unassigned) |
| POST | `/api/v1/agent/tickets/:ticketId/claim` | JWT | agent | Claim an open ticket |
| POST | `/api/v1/agent/tickets/:ticketId/reply` | JWT | agent | Reply to ticket (sends to customer) |
| PATCH | `/api/v1/agent/tickets/:ticketId/status` | JWT | agent | Update ticket status |
| GET | `/api/v1/agent/tickets/:ticketId/messages` | JWT | agent | Get linked ChatSession messages |

### Socket.IO Specification — Agent Real-Time

**Namespace:** `/admin`

**Connection:** `io("wss://domain/admin", { auth: { token: "JWT_TOKEN" } })`

**Authentication:** JWT verified on connection. Agent joins room `company:{companyId}`.

| Event | Direction | Payload | Description |
|-------|-----------|---------|-------------|
| `ticket:watch` | Client → Server | `"ticketId"` | Join ticket-specific room for real-time updates |
| `ticket:sendMessage` | Client → Server | `{ ticketId, content }` | Send message to customer via agent socket |
| `ticket:messageSent` | Server → Client | `{ ticketId, message }` | Confirmation that message was delivered |
| `ticket:messageError` | Server → Client | `{ ticketId, error }` | Message delivery failed |
| `ticket:message:new` | Server → Client | `{ ticketId, ticketNumber, message }` | New message in a watched ticket |
| `ticket:assigned` | Server → Client | `{ ticketId, ticketNumber, agentId, agentName }` | Ticket claimed by an agent |
| `ticket:status:changed` | Server → Client | `{ ticketId, status, updatedBy }` | Ticket status changed |
| `ticket:new` | Server → Client | `{ ticket }` | New ticket created in company |
| `chat:customerMessage` | Server → Client | `{ sessionId, message }` | Customer sent a message (company-wide) |

### Agent Idle Fallback

```
Condition: isAgentHandling === true AND
           last agent message timestamp > 5 minutes ago

Trigger:   Next customer message arrives

Action:    1. Message goes through AI pipeline instead of forwarding to agent
           2. isAgentHandling remains true (agent can still respond)
           3. Logged in console: "Agent idle >5min, falling back to AI"

Purpose:   Prevents customers from being stuck waiting for an inactive agent
```

---

## Cycle 6 - SaaS Billing & Subscription Management

### Prerequisites
- Cycle 1 complete (Company entity)

### Entities Created

**Plan** — See ERD-Documentation.md Section 1

**Subscription** — See ERD-Documentation.md Section 2

**Invoice** — See ERD-Documentation.md Section 3

### API Endpoints

#### Plan Management

| Method | Endpoint | Auth | Roles | Description |
|--------|----------|------|-------|-------------|
| POST | `/api/v1/admin/plans` | JWT | super_admin | Create a pricing plan |
| GET | `/api/v1/admin/plans` | JWT | super_admin | List all plans |
| GET | `/api/v1/admin/plans/:id` | JWT | super_admin | Get plan details |
| PATCH | `/api/v1/admin/plans/:id` | JWT | super_admin | Update plan |
| DELETE | `/api/v1/admin/plans/:id` | JWT | super_admin | Deactivate plan |

**POST /api/v1/admin/plans — Request:**
```json
{
  "name": "Professional",
  "tier": "professional",
  "price": 2999,
  "billingCycle": "monthly",
  "maxAgents": 25,
  "maxSessionsPerMonth": 5000,
  "maxKnowledgeItems": 200,
  "features": ["ai_chat", "telegram", "web_chat", "sentiment", "qa_review", "canned_responses"],
  "channels": ["web", "telegram"],
  "aiEnabled": true
}
```

#### Subscription Management

| Method | Endpoint | Auth | Roles | Description |
|--------|----------|------|-------|-------------|
| POST | `/api/v1/admin/subscriptions` | JWT | super_admin | Create subscription for a company |
| GET | `/api/v1/admin/subscriptions` | JWT | super_admin | List all subscriptions |
| GET | `/api/v1/admin/subscription` | JWT | company_manager | Get my company's subscription |
| PATCH | `/api/v1/admin/subscriptions/:id` | JWT | super_admin | Update subscription status |

#### Invoice Management

| Method | Endpoint | Auth | Roles | Description |
|--------|----------|------|-------|-------------|
| POST | `/api/v1/admin/invoices` | JWT | super_admin | Create invoice (manual or system) |
| GET | `/api/v1/admin/invoices` | JWT | super_admin, company_manager | List invoices (filtered by company for managers) |
| PATCH | `/api/v1/admin/invoices/:id` | JWT | super_admin | Update invoice status (mark as paid) |

### Plan Limit Enforcement Logic

```javascript
// Pseudocode — enforced in service layer before resource creation

async function checkPlanLimit(companyId, resource) {
  const subscription = await Subscription.findOne({ companyId, status: { $in: ['active', 'trial'] } });
  if (!subscription) throw ApiError.forbidden('No active subscription');

  const plan = await Plan.findById(subscription.planId);

  switch (resource) {
    case 'agent':
      const agentCount = await User.countDocuments({ companyId, role: 'agent', isActive: true });
      if (agentCount >= plan.maxAgents) throw ApiError.forbidden(`Agent limit reached (${plan.maxAgents}). Please upgrade.`);
      break;
    case 'session':
      const monthStart = new Date(new Date().getFullYear(), new Date().getMonth(), 1);
      const sessionCount = await ChatSession.countDocuments({ companyId, createdAt: { $gte: monthStart } });
      if (sessionCount >= plan.maxSessionsPerMonth) throw ApiError.forbidden(`Monthly session limit reached (${plan.maxSessionsPerMonth}).`);
      break;
    case 'knowledge':
      const kbCount = await KnowledgeItem.countDocuments({ companyId, isActive: true });
      if (kbCount >= plan.maxKnowledgeItems) throw ApiError.forbidden(`Knowledge base limit reached (${plan.maxKnowledgeItems}).`);
      break;
  }
}
```

### Subscription State Machine

```
     ┌──────────┐   Payment received    ┌──────────┐
     │  TRIAL   │ ─────────────────────> │  ACTIVE  │ <──── Renewed
     │ (14 days)│                        │          │
     └────┬─────┘                        └────┬─────┘
          │                                    │
          │ Trial expired                      │ Invoice overdue >7 days
          │ (no payment)                       │
          ▼                                    ▼
     ┌──────────┐                        ┌──────────┐
     │ EXPIRED  │ <───────────────────── │ PAST_DUE │
     │          │   Overdue >30 days     │ (grace)  │
     └──────────┘                        └──────────┘
                                               │
          ┌──────────┐                         │ Payment received
          │CANCELLED │                         └──────> ACTIVE
          │ (by user)│
          └──────────┘
```

---

## Cycle 7 - Department & Team Management

### Prerequisites
- Cycle 1 complete (Company, User)
- Cycle 5 complete (Agent workspace)

### Entities Created

**Department** — See ERD-Documentation.md Section 5

### Entities Modified

**User** — Added: `departmentId` (ObjectId, ref: Department)

### API Endpoints

| Method | Endpoint | Auth | Roles | Description |
|--------|----------|------|-------|-------------|
| POST | `/api/v1/admin/departments` | JWT | company_manager | Create department |
| GET | `/api/v1/admin/departments` | JWT | company_manager, team_leader | List departments |
| GET | `/api/v1/admin/departments/:id` | JWT | company_manager, team_leader | Get department with members |
| PATCH | `/api/v1/admin/departments/:id` | JWT | company_manager | Update department |
| DELETE | `/api/v1/admin/departments/:id` | JWT | company_manager | Deactivate department |

**POST /api/v1/admin/departments — Request:**
```json
{
  "name": "Technical Support",
  "description": "Handles network and technical issues",
  "managerId": "ObjectId (team_leader)"
}
```

---

## Cycle 8 - Performance Analytics & Quality Assurance

### Prerequisites
- Cycle 4 complete (Tickets with resolution timestamps)
- Cycle 5 complete (Agent activity data)

### Entities Created

**AgentPerformance** — See ERD-Documentation.md Section 17

**Rating** — See ERD-Documentation.md Section 18

**QAReview** — See ERD-Documentation.md Section 19

### API Endpoints

#### Ratings

| Method | Endpoint | Auth | Roles | Description |
|--------|----------|------|-------|-------------|
| POST | `/api/v1/ratings` | JWT/Channel | customer | Submit rating for a resolved ticket |
| GET | `/api/v1/admin/ratings` | JWT | company_manager, team_leader | List all ratings |
| GET | `/api/v1/agent/ratings` | JWT | agent | My received ratings |

**POST /api/v1/ratings — Request:**
```json
{
  "ticketId": "ObjectId",
  "score": 4,
  "comment": "Agent was very helpful, resolved my issue quickly"
}
```

#### QA Reviews

| Method | Endpoint | Auth | Roles | Description |
|--------|----------|------|-------|-------------|
| POST | `/api/v1/admin/qa-reviews` | JWT | company_manager, team_leader | Create QA review |
| GET | `/api/v1/admin/qa-reviews` | JWT | company_manager, team_leader | List QA reviews (filtered) |
| PATCH | `/api/v1/admin/qa-reviews/:id` | JWT | company_manager, team_leader | Update review (scores, status) |
| GET | `/api/v1/agent/qa-reviews` | JWT | agent | My QA reviews |
| PATCH | `/api/v1/agent/qa-reviews/:id/dispute` | JWT | agent | Dispute a review |

**POST /api/v1/admin/qa-reviews — Request:**
```json
{
  "sessionId": "CHAT-1803-9682",
  "ticketId": "ObjectId",
  "agentId": "ObjectId",
  "scores": {
    "communication": 8,
    "accuracy": 9,
    "empathy": 7,
    "resolution": 9,
    "compliance": 10
  },
  "strengths": "Excellent technical knowledge, resolved complex issue efficiently",
  "improvements": "Could show more empathy when customer expressed frustration",
  "notes": "Review of billing dispute conversation"
}
```

#### Performance Analytics

| Method | Endpoint | Auth | Roles | Description |
|--------|----------|------|-------|-------------|
| GET | `/api/v1/admin/analytics/agents/performance` | JWT | company_manager, team_leader | Agent performance data |
| GET | `/api/v1/admin/analytics/agents/leaderboard` | JWT | company_manager | Agent ranking |
| GET | `/api/v1/admin/analytics/kpis` | JWT | company_manager | Company KPI dashboard |
| GET | `/api/v1/agent/performance` | JWT | agent | My performance metrics |

### Daily Performance Snapshot Cron

```javascript
// Runs daily at 23:59 via node-cron
// Calculates daily metrics for every active agent

cron.schedule('59 23 * * *', async () => {
  const today = new Date().toISOString().slice(0, 10); // YYYY-MM-DD
  const startOfDay = new Date(today + 'T00:00:00.000Z');
  const endOfDay = new Date(today + 'T23:59:59.999Z');

  const companies = await Company.find({ isActive: true });

  for (const company of companies) {
    const agents = await User.find({ companyId: company._id, role: 'agent', isActive: true });

    for (const agent of agents) {
      const tickets = await Ticket.find({
        companyId: company._id,
        assignedTo: agent._id,
        updatedAt: { $gte: startOfDay, $lte: endOfDay }
      });

      // Calculate metrics...
      await AgentPerformance.findOneAndUpdate(
        { companyId: company._id, agentId: agent._id, period: today },
        { $set: { ticketsAssigned, ticketsResolved, avgHandleTimeMs, ... } },
        { upsert: true }
      );
    }
  }
});
```

---

## Cycle 9 - Sentiment Analysis & AI Intelligence

### Prerequisites
- Cycle 2 complete (AI engine, ChatSession)
- Cycle 8 complete (performance system)

### Entities Created

**SentimentLog** — See ERD-Documentation.md Section 16

**AIConfig** — See ERD-Documentation.md Section 15

### API Endpoints

#### AI Configuration

| Method | Endpoint | Auth | Roles | Description |
|--------|----------|------|-------|-------------|
| GET | `/api/v1/admin/ai-config` | JWT | company_manager | Get company AI settings |
| PATCH | `/api/v1/admin/ai-config` | JWT | company_manager | Update AI settings |

**PATCH /api/v1/admin/ai-config — Request:**
```json
{
  "provider": "groq",
  "model": "llama-3.3-70b-versatile",
  "systemPrompt": "You are a customer service AI for our telecom company. Always respond in Egyptian Arabic when the customer speaks Arabic...",
  "temperature": 0.3,
  "maxTokens": 512,
  "languages": ["ar", "en"],
  "fallbackBehavior": "escalate"
}
```

#### Sentiment Analytics

| Method | Endpoint | Auth | Roles | Description |
|--------|----------|------|-------|-------------|
| GET | `/api/v1/admin/analytics/sentiment` | JWT | company_manager | Sentiment overview |
| GET | `/api/v1/admin/analytics/sentiment/sessions/:sessionId` | JWT | company_manager | Per-session sentiment timeline |

### Sentiment Analysis Integration

```
Customer Message Received
         │
         ▼
┌─────────────────────────┐
│ AI Pipeline (existing)  │  Process message and generate response
└────────────┬────────────┘
             │
             │ Async (non-blocking)
             ▼
┌─────────────────────────┐
│ Sentiment Analyzer       │
│                          │
│ Input: message.content   │
│ Output: {                │
│   sentiment: "negative", │
│   score: -0.72,          │
│   language: "ar"         │
│ }                        │
└────────────┬─────────────┘
             │
             ▼
┌─────────────────────────┐
│ Save SentimentLog        │  Store for analytics
│                          │
│ If score < -0.7:         │
│   → Emit alert to admin  │
│   → Consider auto-       │
│     escalation            │
└──────────────────────────┘
```

---

## Cycle 10 - Notifications, Tags & Operational Tools

### Prerequisites
- Cycle 5 complete (Agent workspace)
- Cycle 7 complete (Departments)

### Entities Created

**Notification** — See ERD-Documentation.md Section 21

**Tag** — See ERD-Documentation.md Section 11

**TicketTag** — See ERD-Documentation.md Section 12

**CannedResponse** — See ERD-Documentation.md Section 9

**Attachment** — See ERD-Documentation.md Section 13

### API Endpoints

#### Notifications

| Method | Endpoint | Auth | Roles | Description |
|--------|----------|------|-------|-------------|
| GET | `/api/v1/notifications` | JWT | Any staff | My notifications (paginated) |
| GET | `/api/v1/notifications/unread-count` | JWT | Any staff | Unread count for badge |
| PATCH | `/api/v1/notifications/:id/read` | JWT | Any staff | Mark as read |
| PATCH | `/api/v1/notifications/read-all` | JWT | Any staff | Mark all as read |

#### Tags

| Method | Endpoint | Auth | Roles | Description |
|--------|----------|------|-------|-------------|
| POST | `/api/v1/admin/tags` | JWT | company_manager | Create tag |
| GET | `/api/v1/admin/tags` | JWT | Any staff | List tags |
| PATCH | `/api/v1/admin/tags/:id` | JWT | company_manager | Update tag |
| DELETE | `/api/v1/admin/tags/:id` | JWT | company_manager | Delete tag |
| POST | `/api/v1/agent/tickets/:ticketId/tags` | JWT | agent | Add tag to ticket |
| DELETE | `/api/v1/agent/tickets/:ticketId/tags/:tagId` | JWT | agent | Remove tag from ticket |

#### Canned Responses

| Method | Endpoint | Auth | Roles | Description |
|--------|----------|------|-------|-------------|
| POST | `/api/v1/admin/canned-responses` | JWT | company_manager | Create canned response |
| GET | `/api/v1/admin/canned-responses` | JWT | Any staff | List canned responses |
| PATCH | `/api/v1/admin/canned-responses/:id` | JWT | company_manager | Update canned response |
| DELETE | `/api/v1/admin/canned-responses/:id` | JWT | company_manager | Delete canned response |
| GET | `/api/v1/agent/canned-responses` | JWT | agent | List responses (agent view) |

#### Attachments

| Method | Endpoint | Auth | Roles | Description |
|--------|----------|------|-------|-------------|
| POST | `/api/v1/agent/tickets/:ticketId/attachments` | JWT | agent | Upload file to ticket |
| GET | `/api/v1/agent/tickets/:ticketId/attachments` | JWT | agent | List ticket attachments |

### Notification Socket Events

| Event | Direction | Payload | Trigger |
|-------|-----------|---------|---------|
| `notification:new` | Server → Client | `{ _id, type, title, message, data, createdAt }` | New notification created |
| `notification:count` | Server → Client | `{ unreadCount: number }` | Unread count updated |

### Notification Triggers

| Event | Recipients | Notification Type | Title |
|-------|------------|-------------------|-------|
| Ticket assigned to agent | The assigned agent | `ticket_assigned` | "New Ticket Assigned" |
| Ticket resolved | Customer (if has staff account), manager | `ticket_resolved` | "Ticket Resolved" |
| New customer message (agent handling) | Assigned agent | `new_message` | "New Customer Message" |
| QA review completed | Reviewed agent | `qa_review` | "QA Review Available" |
| Performance snapshot ready | Each agent | `performance` | "Daily Performance Summary" |
| System announcement | All staff in company | `system` | Configurable |

---

## Cycle 11 - Admin Dashboard & Reporting

### Prerequisites
- All previous cycles (data must exist)

### API Endpoints

#### Dashboard Overview

| Method | Endpoint | Auth | Roles | Description |
|--------|----------|------|-------|-------------|
| GET | `/api/v1/admin/dashboard` | JWT | company_manager | Real-time company overview |

**Response:**
```json
{
  "data": {
    "activeSessions": 23,
    "openTickets": 15,
    "inProgressTickets": 8,
    "onlineAgents": 5,
    "csatToday": 4.2,
    "avgResponseTimeToday": 32000,
    "aiResolutionRate": 0.42,
    "ticketsCreatedToday": 34,
    "ticketsResolvedToday": 28
  }
}
```

#### Ticket Analytics

| Method | Endpoint | Auth | Roles | Description |
|--------|----------|------|-------|-------------|
| GET | `/api/v1/admin/analytics/tickets` | JWT | company_manager | Ticket volume and distribution |
| GET | `/api/v1/admin/analytics/tickets/trends` | JWT | company_manager | Ticket trends over time |

**GET /api/v1/admin/analytics/tickets — Query Parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `from` | Date | Start date |
| `to` | Date | End date |
| `groupBy` | String | day, week, month |
| `departmentId` | ObjectId | Filter by department |

**Response:**
```json
{
  "data": {
    "total": 450,
    "byStatus": { "open": 15, "in_progress": 8, "resolved": 327, "closed": 100 },
    "byCategory": { "billing": 120, "network": 95, "packages": 80, "complaint": 155 },
    "byChannel": { "telegram": 280, "web": 170 },
    "byPriority": { "low": 50, "medium": 250, "high": 100, "urgent": 50 },
    "avgResolutionTimeMs": 3600000,
    "avgFirstResponseMs": 45000,
    "resolutionRate": 0.95
  }
}
```

#### Chat/AI Analytics

| Method | Endpoint | Auth | Roles | Description |
|--------|----------|------|-------|-------------|
| GET | `/api/v1/admin/analytics/chat` | JWT | company_manager | Chat and AI performance |

**Response:**
```json
{
  "data": {
    "totalSessions": 1200,
    "aiResolvedSessions": 504,
    "aiResolutionRate": 0.42,
    "escalatedSessions": 696,
    "avgSessionMessages": 12,
    "byChannel": { "telegram": 720, "web": 480 },
    "topIntents": [
      { "intent": "billing_inquiry", "count": 340 },
      { "intent": "complaint", "count": 280 },
      { "intent": "package_info", "count": 220 }
    ]
  }
}
```

#### Platform Admin Dashboard

| Method | Endpoint | Auth | Roles | Description |
|--------|----------|------|-------|-------------|
| GET | `/api/v1/admin/platform/dashboard` | JWT | super_admin | Platform-wide stats |
| GET | `/api/v1/admin/platform/companies` | JWT | super_admin | All companies with health status |

**Platform Dashboard Response:**
```json
{
  "data": {
    "totalCompanies": 12,
    "activeCompanies": 10,
    "totalUsers": 340,
    "totalSessions": 15000,
    "totalTickets": 4500,
    "mrr": 45000,
    "subscriptionsByTier": { "free": 2, "starter": 4, "professional": 3, "enterprise": 1 },
    "overdueInvoices": 2,
    "overdueAmount": 5998
  }
}
```

#### Report Export

| Method | Endpoint | Auth | Roles | Description |
|--------|----------|------|-------|-------------|
| GET | `/api/v1/admin/reports/export` | JWT | company_manager | Export report as CSV/PDF |

**Query Parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `type` | String | tickets, agents, csat, qa, sessions |
| `format` | String | csv, pdf |
| `from` | Date | Start date |
| `to` | Date | End date |
| `departmentId` | ObjectId | Optional filter |

### MongoDB Aggregation Pipeline Examples

**Ticket Analytics Aggregation:**
```javascript
Ticket.aggregate([
  { $match: { companyId, createdAt: { $gte: from, $lte: to } } },
  {
    $group: {
      _id: { status: '$status' },
      count: { $sum: 1 },
      avgResolution: {
        $avg: {
          $cond: [
            { $ne: ['$resolvedAt', null] },
            { $subtract: ['$resolvedAt', '$createdAt'] },
            null
          ]
        }
      }
    }
  }
]);
```

**Agent Leaderboard Aggregation:**
```javascript
AgentPerformance.aggregate([
  { $match: { companyId, period: { $gte: fromDate, $lte: toDate } } },
  {
    $group: {
      _id: '$agentId',
      totalResolved: { $sum: '$ticketsResolved' },
      avgCsat: { $avg: '$csatAvg' },
      avgHandleTime: { $avg: '$avgHandleTimeMs' },
      avgFCR: { $avg: '$firstCallResolutionRate' }
    }
  },
  { $sort: { totalResolved: -1 } },
  {
    $lookup: {
      from: 'users',
      localField: '_id',
      foreignField: '_id',
      as: 'agent'
    }
  }
]);
```

---

## Cycle 12 - Testing, Security & Production Launch

### Prerequisites
- All previous cycles complete

### Security Requirements

| # | Requirement | Implementation |
|---|-------------|---------------|
| SEC-1 | Input validation on all endpoints | Joi schemas with stripUnknown, type checking, length limits |
| SEC-2 | NoSQL injection prevention | Mongoose parameterized queries, no raw `$where` or `$eval` |
| SEC-3 | XSS prevention | Helmet.js for Content-Security-Policy, output sanitization |
| SEC-4 | Authentication on all protected routes | JWT verification via `protect` middleware |
| SEC-5 | Authorization (RBAC) | `requirePermission` / `allowRoles` on every route |
| SEC-6 | Multi-tenant data isolation | `companyId` filter on every database query |
| SEC-7 | Password security | bcrypt 12 rounds, min 6 chars, never returned in responses |
| SEC-8 | Rate limiting | express-rate-limit: 100 req/15min on auth, 1000 req/15min on API |
| SEC-9 | HTTPS enforcement | TLS 1.2+ in production, HSTS header via Helmet |
| SEC-10 | CORS configuration | Whitelist allowed origins, no wildcards in production |
| SEC-11 | File upload security | Type validation (images only for profiles), size limits, random filenames |
| SEC-12 | Environment secrets | All secrets in .env, never committed to git |

### Testing Strategy

| Level | Scope | Tool | Coverage Target |
|-------|-------|------|----------------|
| Unit Tests | Services, utilities, helpers | Jest / Vitest | >80% line coverage |
| Integration Tests | API endpoints (full request-response) | Supertest + Jest | All routes tested |
| E2E Tests | Critical user flows | Custom scripts | 5 critical flows |
| Load Tests | Performance under concurrent users | Artillery / k6 | 100 concurrent users, <200ms p95 |

### Critical E2E Test Flows

| # | Flow | Steps |
|---|------|-------|
| E2E-1 | Customer to AI Resolution | Telegram message → AI response → Customer satisfied → Session closed |
| E2E-2 | Full Escalation Flow | Customer message → AI can't resolve → Ticket created → Agent claims → Agent replies (delivered to Telegram) → Ticket resolved → Rating submitted |
| E2E-3 | Agent Real-Time Chat | Agent connects socket → Watches ticket → Customer sends message → Agent sees it instantly → Agent replies via socket → Customer receives on Telegram |
| E2E-4 | SaaS Lifecycle | Plan created → Company subscribed (trial) → Agents added → Sessions created → Plan limit enforced → Invoice generated → Payment recorded |
| E2E-5 | Multi-Tenant Isolation | Two companies created → Agent from Company A tries to access Company B's ticket → 403 Forbidden |

### Production Deployment

```yaml
# Docker Compose Structure
services:
  natiq-api:
    build: .
    ports:
      - "5000:5000"
    env_file: .env
    depends_on:
      - mongodb
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:5000/health"]
      interval: 30s
      timeout: 10s
      retries: 3

  mongodb:
    image: mongo:6
    volumes:
      - mongo-data:/data/db
    ports:
      - "27017:27017"

volumes:
  mongo-data:
```

### Health Check Endpoint

```
GET /health

Response (200):
{
  "status": "ok",
  "uptime": 86400,
  "database": "connected",
  "timestamp": "2026-02-13T00:00:00.000Z",
  "version": "1.0.0"
}
```

---

## Non-Functional Requirements

### Performance

| # | Requirement | Target |
|---|-------------|--------|
| NFR-P1 | API response time (list endpoints) | <200ms (p95) |
| NFR-P2 | API response time (single resource) | <100ms (p95) |
| NFR-P3 | WebSocket message delivery | <50ms (server to client) |
| NFR-P4 | AI response generation | <3000ms (including LLM call) |
| NFR-P5 | Concurrent WebSocket connections | 500+ per server |
| NFR-P6 | Database query performance | No full collection scans on paginated endpoints |

### Scalability

| # | Requirement | Target |
|---|-------------|--------|
| NFR-S1 | Companies supported | 100+ on single instance |
| NFR-S2 | Users per company | Up to 500 |
| NFR-S3 | Chat sessions per month (platform) | 100,000+ |
| NFR-S4 | Message storage per session | Up to 500 messages |
| NFR-S5 | Horizontal scaling | Stateless API design, Socket.IO with Redis adapter when needed |

### Availability

| # | Requirement | Target |
|---|-------------|--------|
| NFR-A1 | Uptime SLA | 99.5% (4.38 hours downtime/year max) |
| NFR-A2 | Graceful shutdown | Complete in-flight requests before stopping |
| NFR-A3 | Database connection recovery | Auto-reconnect on connection loss |
| NFR-A4 | External API failures (Groq, Telegram) | Fallback behavior, no cascading failure |

### Maintainability

| # | Requirement | Target |
|---|-------------|--------|
| NFR-M1 | Code structure | MVC with service layer (controller → service → model) |
| NFR-M2 | Error handling | Centralized via asyncHandler and ApiError classes |
| NFR-M3 | Logging | Structured JSON logs with request correlation IDs |
| NFR-M4 | Documentation | API documented via SRS + Postman collection |

---

## External Interfaces

### Groq LLM API

| Field | Value |
|-------|-------|
| Base URL | `https://api.groq.com/openai/v1` |
| Authentication | `Authorization: Bearer GROQ_API_KEY` |
| Endpoint Used | `/chat/completions` |
| Default Model | `llama-3.3-70b-versatile` |
| Rate Limit | Depends on Groq plan (free tier: 30 req/min) |
| Fallback | Return friendly message + create ticket on failure |

### Telegram Bot API

| Field | Value |
|-------|-------|
| Base URL | `https://api.telegram.org/bot{token}` |
| Authentication | Bot token in URL path |
| Endpoints Used | `/sendMessage`, `/setWebhook`, `/getWebhookInfo` |
| Webhook | Natiq receives POST requests from Telegram at `/api/v1/channels/telegram/:slug/webhook` |
| Rate Limit | 30 messages/second to same chat, 20 messages/minute to same group |
| Fallback | Retry without Markdown parse_mode on failure |

### MongoDB

| Field | Value |
|-------|-------|
| Connection | `MONGODB_URI` environment variable |
| Driver | Mongoose 7.x+ |
| Indexes | Defined per model, compound indexes for common query patterns |
| Transactions | Used for atomic operations where needed (ticket claiming) |
