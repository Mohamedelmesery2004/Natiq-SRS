# Natiq - Business Requirements Document (BRD)

| Field | Value |
|-------|-------|
| **Document Title** | Business Requirements Document (BRD) |
| **Project Name** | Natiq - AI-Powered Omnichannel Contact Center SaaS |
| **Version** | 1.0 |
| **Date** | February 2026 |
| **Status** | In Progress |
| **Confidentiality** | Internal Use Only |

---

## Table of Contents

1. [Executive Summary](#1-executive-summary)
2. [Business Objectives](#2-business-objectives)
3. [Problem Statements](#3-problem-statements)
4. [Stakeholders](#4-stakeholders)
5. [Target Customer Segments](#5-target-customer-segments)
6. [Revenue Model](#6-revenue-model)
7. [KPIs & Success Criteria](#7-kpis--success-criteria)
8. [Development Cycles Overview](#8-development-cycles-overview)
9. [Cycle 1 - Foundation & Multi-Tenant Core](#cycle-1---foundation--multi-tenant-core)
10. [Cycle 2 - AI Chat Engine & Knowledge Base](#cycle-2---ai-chat-engine--knowledge-base)
11. [Cycle 3 - Omnichannel Integration](#cycle-3---omnichannel-integration)
12. [Cycle 4 - Ticket Management System](#cycle-4---ticket-management-system)
13. [Cycle 5 - Agent Workspace](#cycle-5---agent-workspace)
14. [Cycle 6 - SaaS Billing & Subscription Management](#cycle-6---saas-billing--subscription-management)
15. [Cycle 7 - Department & Team Management](#cycle-7---department--team-management)
16. [Cycle 8 - Performance Analytics & Quality Assurance](#cycle-8---performance-analytics--quality-assurance)
17. [Cycle 9 - Sentiment Analysis & AI Intelligence](#cycle-9---sentiment-analysis--ai-intelligence)
18. [Cycle 10 - Notifications, Tags & Operational Tools](#cycle-10---notifications-tags--operational-tools)
19. [Cycle 11 - Admin Dashboard & Reporting](#cycle-11---admin-dashboard--reporting)
20. [Cycle 12 - Testing, Security & Production Launch](#cycle-12---testing-security--production-launch)
21. [Assumptions & Constraints](#assumptions--constraints)
22. [Risks & Mitigation](#risks--mitigation)
23. [Glossary](#glossary)

---

## 1. Executive Summary

Natiq is an AI-powered omnichannel contact center SaaS platform designed to solve critical problems facing call centers in the Middle East and North Africa (MENA) region: excessive call pressure, limited quality monitoring, agent burnout, and language barriers (especially Arabic dialect support).

The platform provides:
- AI chatbot handling routine inquiries across multiple channels (Web, Telegram, WhatsApp)
- Intelligent escalation to human agents when AI cannot resolve
- Real-time agent performance monitoring and quality assurance
- Knowledge base with RAG (Retrieval-Augmented Generation) for accurate AI responses
- Multi-tenant architecture allowing multiple companies to use the platform independently

Natiq differentiates from competitors (CloudTalk, Ziwo, Widebot, Tactful AI, Awfar) through:
- Egyptian Arabic dialect support (competitors lack this)
- 100% conversation review via AI (vs 2-5% manual sampling industry standard)
- Combined voice + chat analytics (competitors do one or the other)
- Agent performance coaching system
- Local pricing for the MENA market

---

## 2. Business Objectives

| # | Objective | Measurable Target |
|---|-----------|-------------------|
| BO-1 | Reduce call center call pressure | 40% of inquiries handled by AI without human intervention |
| BO-2 | Improve customer satisfaction | CSAT score increase by +15% within 3 months of deployment |
| BO-3 | Reduce operational costs | 18% cost reduction in Year 1 |
| BO-4 | Decrease average handle time | AHT reduced by 25% within 6 months |
| BO-5 | Achieve high first-call resolution | FCR rate of 85%+ |
| BO-6 | Scale as SaaS product | Onboard 10+ companies in first year |
| BO-7 | Support Arabic-first communication | Full Egyptian Arabic dialect support in AI and UI |

---

## 3. Problem Statements

| # | Problem | Evidence | Impact |
|---|---------|----------|--------|
| P-1 | Too many disconnected communication channels | Harvard Business Review: employees spend 28% of time on emails alone | Slow response, information lost between channels, customer frustration |
| P-2 | Excessive hotline call pressure | NTRA 2023: 322K+ complaints, 81% via phone, avg resolution >2.5 days | Long wait times, abandoned calls, customer churn |
| P-3 | Manual quality monitoring is limited | McKinsey/CMSWire: AI voice analytics improves QA efficiency by 5x | Only 2-5% of interactions manually reviewed, systemic issues missed |
| P-4 | Agent burnout and poor training | Industry data: 30-45% annual turnover in call centers | High recruitment costs, inconsistent service quality, knowledge loss |

---

## 4. Stakeholders

| Role | Stakeholder | Interest |
|------|-------------|----------|
| Platform Owner | Natiq Team (platform_super_admin) | Full system control, onboard companies, monitor platform health |
| Company Manager | Client company decision makers (company_manager) | Configure their account, manage staff, view analytics, manage knowledge base |
| Team Leader | Department supervisors (team_leader) | Manage their team's agents, conduct QA reviews, monitor team performance |
| Agent | Customer service representatives (agent) | Handle tickets, reply to customers, view their performance metrics |
| Customer | End-users seeking support (customer) | Get issues resolved quickly across their preferred channel |

---

## 5. Target Customer Segments

| Segment | Description | Examples |
|---------|-------------|----------|
| Large Enterprises | Telecom, banking, utilities with 50+ agents | Vodafone Egypt, Orange, CIB |
| SMEs | Small-medium businesses with 5-20 agents | E-commerce stores, clinics, service providers |
| BPO Companies | Outsourced call center providers | Third-party support companies |
| Government Institutions | Public service organizations | NTRA, social services, municipal offices |
| Training Centers | Organizations training call center staff | Customer service academies |

---

## 6. Revenue Model

| Stream | Type | Description |
|--------|------|-------------|
| Subscription Plans | Recurring | Monthly/annual SaaS fees (Free, Starter, Professional, Enterprise) |
| Pay-per-use | Variable | Overage charges when exceeding plan limits (sessions, agents) |
| Setup Fees | One-time | Initial onboarding, configuration, and customization |
| Paid Training | One-time | Agent training programs using AI simulation |
| Advanced Analytics | Add-on | Premium reporting, sentiment trends, custom dashboards |
| API Integration | Add-on | Custom integrations with client's existing CRM/ERP systems |

---

## 7. KPIs & Success Criteria

| KPI | Baseline | Target | Timeline |
|-----|----------|--------|----------|
| Average Handle Time (AHT) | Current industry avg | 25% reduction | 6 months |
| Customer Satisfaction (CSAT) | Current baseline | +15% improvement | 3 months |
| Operational Cost | Current cost | 18% reduction | Year 1 |
| First Call Resolution (FCR) | Industry avg ~70% | 85%+ | 6 months |
| AI Resolution Rate | 0% (no AI) | 40% without human | 3 months |
| Agent Turnover | 30-45% annually | 20% reduction | Year 1 |
| QA Coverage | 2-5% manual | 100% automated | 3 months |

---

## 8. Development Cycles Overview

| Cycle | Name | Duration | Status |
|-------|------|----------|--------|
| 1 | Foundation & Multi-Tenant Core | 2 weeks | Completed |
| 2 | AI Chat Engine & Knowledge Base | 2 weeks | Completed |
| 3 | Omnichannel Integration | 2 weeks | Completed |
| 4 | Ticket Management System | 2 weeks | Completed |
| 5 | Agent Workspace | 2 weeks | Completed |
| 6 | SaaS Billing & Subscription Management | 2 weeks | Not Started |
| 7 | Department & Team Management | 1 week | Not Started |
| 8 | Performance Analytics & Quality Assurance | 2 weeks | Not Started |
| 9 | Sentiment Analysis & AI Intelligence | 2 weeks | Not Started |
| 10 | Notifications, Tags & Operational Tools | 1 week | Not Started |
| 11 | Admin Dashboard & Reporting | 2 weeks | Not Started |
| 12 | Testing, Security & Production Launch | 2 weeks | Not Started |

---

## Cycle 1 - Foundation & Multi-Tenant Core

### Cycle Information

| Field | Detail |
|-------|--------|
| **Cycle Name** | Foundation & Multi-Tenant Core |
| **Cycle ID** | CYC-01 |
| **Goal** | Establish the base architecture: multi-tenant data isolation, user authentication, role-based access control, and API foundation |
| **Business Value** | Without multi-tenancy, Natiq cannot serve multiple companies on the same platform — the entire SaaS model depends on this. Without RBAC, there is no security boundary between roles |
| **Prerequisites** | None (greenfield project) |
| **Entities Involved** | Company, User |
| **Depends On** | - |
| **Duration** | 2 weeks |

### User Stories

| ID | Role | Story | Acceptance Criteria | Priority |
|----|------|-------|---------------------|----------|
| US-1.1 | Platform Super Admin | As a platform super admin, I want to create new companies on the platform so that new clients can be onboarded | - API endpoint POST /api/v1/admin/companies creates a company with name, slug, industry - Slug must be unique - Company is created with default channelsConfig and settings - Response returns the created company data | Must Have |
| US-1.2 | Platform Super Admin | As a platform super admin, I want to create users for any company so that I can set up initial staff accounts | - API endpoint POST /api/v1/admin/users creates a user with companyId, name, email, role, password - Email must be unique per company - Password is hashed with bcrypt (12 rounds) - Valid roles: platform_super_admin, company_manager, team_leader, agent, customer | Must Have |
| US-1.3 | Company Manager | As a company manager, I want to log in with my email and password so that I can access my company's dashboard | - API endpoint POST /api/v1/admin/auth/login accepts email, password, companySlug - Returns JWT token valid for 7 days - Returns user profile (without passwordHash) - Invalid credentials return 401 Unauthorized | Must Have |
| US-1.4 | Any Staff User | As a staff user, I want my API requests to be scoped to my company only so that I cannot see or modify another company's data | - Every protected endpoint includes companyId from the authenticated user's token - All database queries filter by companyId - An agent at Company A cannot access Company B's tickets, users, or sessions | Must Have |
| US-1.5 | Company Manager | As a company manager, I want to manage my company's staff (CRUD) so that I can add agents, team leaders, and update their information | - GET /api/v1/admin/users lists all users for the company - PATCH /api/v1/admin/users/:id updates user fields - DELETE /api/v1/admin/users/:id deactivates the user (soft delete) - Managers cannot manage users of other companies | Must Have |
| US-1.6 | Any Staff User | As a staff user, I want my permissions to be checked on every request so that I can only perform actions allowed by my role | - RBAC matrix enforced via requirePermission middleware - Agent can read tickets but not delete them - Customer can read their tickets but not update - team_leader can create agents but not delete companies | Must Have |
| US-1.7 | Platform Super Admin | As a platform super admin, I want to view and manage all companies so that I can monitor the platform | - GET /api/v1/admin/companies returns all companies - PATCH /api/v1/admin/companies/:id updates company settings - Can deactivate a company (isActive = false) | Must Have |

---

## Cycle 2 - AI Chat Engine & Knowledge Base

### Cycle Information

| Field | Detail |
|-------|--------|
| **Cycle Name** | AI Chat Engine & Knowledge Base |
| **Cycle ID** | CYC-02 |
| **Goal** | Build the AI-powered chat engine that answers customer questions using a knowledge base with RAG (Retrieval-Augmented Generation), including session management and intelligent escalation |
| **Business Value** | This is the core product — AI handling 40% of inquiries without human intervention. The knowledge base enables accurate, company-specific answers instead of generic AI responses. Directly addresses P-2 (call pressure) and BO-1 (AI resolution) |
| **Prerequisites** | Cycle 1 (Company and User entities, authentication) |
| **Entities Involved** | ChatSession, Message (embedded), KnowledgeItem |
| **Depends On** | CYC-01 |
| **Duration** | 2 weeks |

### User Stories

| ID | Role | Story | Acceptance Criteria | Priority |
|----|------|-------|---------------------|----------|
| US-2.1 | Company Manager | As a company manager, I want to add knowledge base items (FAQs, packages, policies) so that the AI can answer customers accurately about our specific products and services | - API endpoints for CRUD on /api/v1/admin/knowledge - Types: package, faq, policy, complaint_flow - Each item has title, content, features[], slug - Items are scoped to companyId - Slug is unique per company | Must Have |
| US-2.2 | Company Manager | As a company manager, I want knowledge base items to be embedded as vectors so that the AI can find the most relevant items for each customer question | - POST /api/v1/admin/embeddings/generate triggers embedding generation - Each KnowledgeItem gets an embeddingVector using the configured embedding model - embeddingModel and lastEmbeddedAt are updated - Semantic search finds relevant items by cosine similarity | Must Have |
| US-2.3 | Customer | As a customer, I want to send a message and receive an AI-generated response based on the company's knowledge base so that I get accurate answers without waiting for a human | - Customer message is received and stored in ChatSession - System finds relevant KnowledgeItems via RAG (semantic search) - LLM generates a response using the relevant knowledge as context - Response is stored in session and returned to customer - If no relevant knowledge is found, AI responds with general assistance | Must Have |
| US-2.4 | Customer | As a customer, I want the AI to detect when it cannot help me and automatically create a support ticket so that a human agent can assist me | - AI evaluates confidence score for each response - If confidence < escalationThreshold (company setting), escalation triggers - Escalation keywords detected: "agent", "human", "ticket", "complain" - A Ticket is created with context (sessionId, lastUserMessage, aiSummary) - Customer is notified that a ticket has been created - session.summary.linkedTicketId is set | Must Have |
| US-2.5 | Customer | As a customer, I want my conversation history to persist within a session so that I don't have to repeat myself | - All messages stored in ChatSession.messages array - AI receives recent message history as context for each response - Session tracks messageCount and lastActivity - Session has a maximum message limit (company.settings.maxSessionMessages) | Must Have |
| US-2.6 | Company Manager | As a company manager, I want to view all chat sessions for my company so that I can monitor AI performance and customer interactions | - GET /api/v1/admin/chat/sessions returns paginated sessions - Filterable by channel, status, date range - Each session includes message count, channel, status, and customer info - Can view full message history for a session | Should Have |
| US-2.7 | Customer | As a customer, I want the AI to understand Arabic (Egyptian dialect) so that I can communicate naturally in my language | - AI system prompt includes instruction to respond in customer's language - Arabic messages are processed and responded to in Arabic - Knowledge base items can be in Arabic - Detected language is tracked for analytics | Must Have |

---

## Cycle 3 - Omnichannel Integration

### Cycle Information

| Field | Detail |
|-------|--------|
| **Cycle Name** | Omnichannel Integration |
| **Cycle ID** | CYC-03 |
| **Goal** | Enable customers to reach Natiq through multiple communication channels (Telegram, Web Chat) with unified conversation management. Each channel delivers messages to the same AI engine and stores them in the same ChatSession model |
| **Business Value** | "Omnichannel" is Natiq's core value proposition and primary differentiator against competitors. Customers choose their preferred channel; the company sees all conversations in one place. Directly addresses P-1 (disconnected channels) |
| **Prerequisites** | Cycle 1 (User/Company), Cycle 2 (ChatSession, AI Engine) |
| **Entities Involved** | ChatSession, User (telegramChatId, onboardingStep) |
| **Depends On** | CYC-01, CYC-02 |
| **Duration** | 2 weeks |

### User Stories

| ID | Role | Story | Acceptance Criteria | Priority |
|----|------|-------|---------------------|----------|
| US-3.1 | Customer | As a Telegram user, I want to message the company's bot and receive AI responses so that I can get support without downloading another app | - Telegram webhook endpoint: POST /api/v1/channels/telegram/:companySlug/webhook - Incoming Telegram messages are processed through the AI pipeline - AI responses are sent back via Telegram Bot API - Messages stored in ChatSession with channel: "telegram" | Must Have |
| US-3.2 | Customer | As a first-time Telegram user, I want to be onboarded (asked for my name and phone) so that the company knows who I am | - /start command triggers onboarding flow - Step 1: Ask for full name, create User with telegramChatId - Step 2: Ask for phone number - Step 0: Onboarding complete, start normal chat - Onboarding state persisted in user.onboardingStep | Must Have |
| US-3.3 | Customer | As a Telegram user, I want to type /start to begin a fresh conversation so that old context doesn't confuse new inquiries | - /start closes any existing active ChatSession (status: closed, endedAt set) - Next message creates a new ChatSession automatically - Welcome message is sent to the user - Old session data is preserved for history | Must Have |
| US-3.4 | Customer | As a Telegram user, I want to type /reset to clear my conversation and start over so that I can ask about a different topic | - /reset closes the current active session - isAgentHandling is reset to false - Confirmation message sent to user - New session created on next message | Should Have |
| US-3.5 | Customer | As a web visitor, I want to chat with the AI through a web widget so that I can get support directly on the company's website | - Socket.IO /webchat namespace for real-time web chat - Customer connects and sends messages via socket events - AI responses emitted back via socket - Messages stored in ChatSession with channel: "web" | Must Have |
| US-3.6 | Company Manager | As a company manager, I want to configure my Telegram bot token so that messages come to my company's bot, not a shared one | - Company.channelsConfig.telegram.botToken stores per-company bot token - Webhook URL uses company slug for routing - Each company's Telegram messages are isolated | Must Have |
| US-3.7 | Customer | As a customer, I want the system to handle message delivery failures gracefully so that I always get a response | - Telegram Markdown parse failures retry without parse_mode - Socket disconnections are handled with reconnection - AI failures return a friendly fallback message instead of an error | Should Have |

---

## Cycle 4 - Ticket Management System

### Cycle Information

| Field | Detail |
|-------|--------|
| **Cycle Name** | Ticket Management System |
| **Cycle ID** | CYC-04 |
| **Goal** | Build the complete ticket lifecycle: creation (by AI escalation or manually), assignment, status management, and resolution tracking. Tickets are the unit of work that bridge AI-handled conversations with human agent intervention |
| **Business Value** | Every customer issue that AI cannot resolve becomes a ticket. Without this system, escalated issues are lost. Ticket metrics (resolution time, first response time) directly feed into KPIs: AHT and FCR |
| **Prerequisites** | Cycle 2 (ChatSession for linked context), Cycle 3 (channel info for routing responses back) |
| **Entities Involved** | Ticket, AgentNote (embedded), EventLog |
| **Depends On** | CYC-02, CYC-03 |
| **Duration** | 2 weeks |

### User Stories

| ID | Role | Story | Acceptance Criteria | Priority |
|----|------|-------|---------------------|----------|
| US-4.1 | System (AI) | As the AI system, I want to automatically create a ticket when escalation is triggered so that no customer issue is lost | - Ticket created with: companyId, userId, channel, category (AI-detected), priority (medium default) - context.sessionId links to the originating ChatSession - context.lastUserMessage and context.aiSummary provide agent with context - ticketNumber generated: NQ-YYYYMMDD-NNNN (sequential per company per day) - session.summary.linkedTicketId updated - EventLog entry: ticket_created | Must Have |
| US-4.2 | Agent | As an agent, I want to see all unassigned tickets for my company so that I can pick up work | - GET /api/v1/agent/tickets?status=open returns unassigned tickets - Sorted by priority (urgent first) then by creation date (oldest first) - Each ticket includes customer name, category, priority, channel, and AI summary - Only shows tickets for the agent's company | Must Have |
| US-4.3 | Agent | As an agent, I want to claim an unassigned ticket so that I become responsible for resolving it | - POST /api/v1/agent/tickets/:ticketId/claim - Atomic operation: findOneAndUpdate with {assignedTo: null, status: open} prevents race conditions - Status changes to in_progress, assignedTo set to agent's ID - Customer notified on their channel: "Agent [name] has picked up your ticket" - EventLog entry: ticket_claimed - Socket event: ticket:assigned broadcast to admin namespace | Must Have |
| US-4.4 | Agent | As an agent, I want to reply to a ticket and have my message delivered to the customer on their original channel so that the customer doesn't need to switch platforms | - POST /api/v1/agent/tickets/:ticketId/reply with content - Message saved to ticket.agentNotes AND to linked ChatSession.messages (role: agent) - If channel is Telegram: sent via Telegram Bot API to customer's chatId - If channel is web: emitted via Socket.IO to customer's session room - firstResponseAt set on first reply - EventLog entry: agent_replied | Must Have |
| US-4.5 | Agent | As an agent, I want to resolve a ticket so that it's marked as complete and metrics are recorded | - PATCH /api/v1/agent/tickets/:ticketId/status with status: resolved - resolvedAt timestamp set - Customer notified: "Your ticket has been resolved" - Socket event: ticket:status:changed broadcast - EventLog entry: ticket_resolved | Must Have |
| US-4.6 | Company Manager | As a company manager, I want to view all tickets with filtering so that I can monitor support operations | - GET /api/v1/admin/tickets returns paginated tickets - Filterable by: status, category, priority, assignedTo, channel, date range - Sortable by: createdAt, priority, status - Includes customer and agent details (populated) | Must Have |
| US-4.7 | Customer | As a customer, I want to be able to create a new ticket even if I had one before that was already resolved so that my new issue is tracked separately | - If session.summary.linkedTicketId points to a resolved/closed ticket, allow new ticket creation - New ticket gets its own ticketNumber - session.summary.linkedTicketId updated to new ticket - Previous ticket history preserved | Must Have |
| US-4.8 | Company Manager | As a company manager, I want to manually update ticket priority and category so that I can correct AI classifications | - PATCH /api/v1/admin/tickets/:id updates priority, category, status - EventLog entry: ticket_updated with changes in metadata - Socket event broadcast for real-time dashboard updates | Should Have |

---

## Cycle 5 - Agent Workspace

### Cycle Information

| Field | Detail |
|-------|--------|
| **Cycle Name** | Agent Workspace |
| **Cycle ID** | CYC-05 |
| **Goal** | Build the dedicated agent experience: separate login, personal dashboard, profile management, real-time chat with customers via Socket.IO, and agent-specific views. Agents need a streamlined workspace focused on resolving tickets efficiently |
| **Business Value** | Agents are the primary workforce of every call center. A dedicated, efficient workspace directly impacts AHT (25% reduction target) and agent satisfaction (reducing turnover from 30-45%). Real-time communication eliminates the delay of page refreshes, which is critical for customer experience |
| **Prerequisites** | Cycle 3 (Socket.IO infrastructure), Cycle 4 (Ticket system to claim and reply) |
| **Entities Involved** | User (agent role), Ticket, ChatSession |
| **Depends On** | CYC-03, CYC-04 |
| **Duration** | 2 weeks |

### User Stories

| ID | Role | Story | Acceptance Criteria | Priority |
|----|------|-------|---------------------|----------|
| US-5.1 | Agent | As an agent, I want a dedicated login endpoint so that my authentication is separate from the admin panel | - POST /api/v1/agent/auth/login with email, password, companySlug - Only users with role "agent" can log in here - Returns JWT token and agent profile - Last login timestamp updated | Must Have |
| US-5.2 | Agent | As an agent, I want to see my personal dashboard with key stats so that I know my workload at a glance | - GET /api/v1/agent/dashboard returns: assignedTickets count, resolvedToday count, avgResponseTime, openTickets in queue - Stats calculated in real-time from ticket data - Scoped to agent's companyId and userId | Must Have |
| US-5.3 | Agent | As an agent, I want to update my profile (name, phone, image) so that customers and managers see my current info | - PATCH /api/v1/agent/profile accepts multipart/form-data - profileImage uploaded via multer (jpeg/jpg/png/gif/webp, max 5MB) - Profile image served statically at /uploads/filename - Optional fields: name, phone, password (requires currentPassword) | Should Have |
| US-5.4 | Agent | As an agent, I want to receive real-time customer messages via Socket.IO so that I can respond instantly without refreshing | - Agent connects to /admin namespace with JWT token - Agent emits ticket:watch with ticketId to join ticket room - Customer messages appear as ticket:message:new events instantly - No polling or page refresh needed | Must Have |
| US-5.5 | Agent | As an agent, I want to send messages via Socket.IO so that my replies reach the customer instantly | - Agent emits ticket:sendMessage with {ticketId, content} - Message saved to ChatSession and ticket agentNotes - Delivered to customer on their channel (Telegram/web) - Agent receives ticket:messageSent confirmation - Other agents watching the same ticket see ticket:message:new | Must Have |
| US-5.6 | Agent | As an agent, I want to load the full chat history for a ticket so that I have context before responding | - GET /api/v1/agent/tickets/:ticketId/messages - Returns all messages from the linked ChatSession - Includes customer name, channel, session status - Shows messages from all roles (user, assistant, agent, system) | Must Have |
| US-5.7 | System | As the system, I want to fall back to AI when an agent is idle for more than 5 minutes so that customers are not left waiting | - If isAgentHandling is true but agent hasn't responded in 5 minutes - Next customer message bypasses agent and goes to AI pipeline - Prevents customers from being stuck in limbo | Must Have |
| US-5.8 | System | As the system, I want to throttle the "forwarded to agent" acknowledgment to once per 30 minutes so that customers are not spammed with repeated messages | - When customer sends a message during agent handling, check last agent_ack timestamp - If >30 minutes since last ack, send acknowledgment and log it - If <30 minutes, silently forward without ack message | Should Have |
| US-5.9 | Company Manager | As a company manager, I want to see an overview of all agents' activity so that I can monitor workforce utilization | - GET /api/v1/admin/analytics/agents/overview - Returns per-agent stats: name, activeTickets, resolvedToday, avgResponseTime, status - Sortable and filterable by team/department | Should Have |

---

## Cycle 6 - SaaS Billing & Subscription Management

### Cycle Information

| Field | Detail |
|-------|--------|
| **Cycle Name** | SaaS Billing & Subscription Management |
| **Cycle ID** | CYC-06 |
| **Goal** | Implement the subscription-based revenue model: pricing plans with feature limits, company subscriptions with lifecycle management, and invoice generation. This transforms Natiq from a single-deployment tool into a scalable SaaS business |
| **Business Value** | Without billing, there is no revenue. The Business Model document defines multiple revenue streams (subscriptions, setup fees, add-ons, usage-based). This cycle enables: free trials to acquire customers, plan-based feature gating to drive upgrades, and invoice tracking for financial management |
| **Prerequisites** | Cycle 1 (Company entity as the subscription owner) |
| **Entities Involved** | Plan, Subscription, Invoice |
| **Depends On** | CYC-01 |
| **Duration** | 2 weeks |

### User Stories

| ID | Role | Story | Acceptance Criteria | Priority |
|----|------|-------|---------------------|----------|
| US-6.1 | Platform Super Admin | As a platform super admin, I want to create and manage pricing plans so that companies can subscribe to different service tiers | - CRUD endpoints for /api/v1/admin/plans - Each plan: name, tier, price, billingCycle, maxAgents, maxSessionsPerMonth, features[], channels[] - Tiers: free, starter, professional, enterprise - Deactivating a plan prevents new subscriptions but doesn't affect existing ones | Must Have |
| US-6.2 | Platform Super Admin | As a platform super admin, I want to assign a subscription to a company so that they can start using the platform based on their selected plan | - POST /api/v1/admin/subscriptions creates subscription for a company - Links companyId to planId with status (trial/active) - trial status auto-set with trialEndsAt (14 days default) - Active subscription enables full platform access based on plan limits | Must Have |
| US-6.3 | System | As the system, I want to enforce plan limits so that companies cannot exceed their subscribed capacity | - When creating a new agent user, check against plan.maxAgents - When creating a new chat session, check against plan.maxSessionsPerMonth - When adding knowledge items, check against plan.maxKnowledgeItems - If limit exceeded, return 403 with message: "Plan limit reached. Please upgrade." - Feature flags in plan.features control access to premium features (sentiment, QA, etc.) | Must Have |
| US-6.4 | System | As the system, I want to generate invoices automatically for each billing cycle so that payment tracking is automated | - On subscription renewal date, create Invoice with type: subscription - invoiceNumber generated sequentially: INV-YYYY-NNNN - Invoice includes line items derived from plan pricing - Status set to pending, dueDate set to 15 days from generation | Must Have |
| US-6.5 | Platform Super Admin | As a platform super admin, I want to mark invoices as paid so that subscription status stays current | - PATCH /api/v1/admin/invoices/:id with status: paid - paidAt timestamp recorded - If overdue invoices exist, company gets warning notifications - After grace period, expired subscription restricts access | Must Have |
| US-6.6 | Company Manager | As a company manager, I want to view my subscription details and invoice history so that I know what I'm paying for and when | - GET /api/v1/admin/subscription returns current subscription with plan details - GET /api/v1/admin/invoices returns paginated invoice history - Each invoice shows amount, status, items, dueDate, paidAt | Should Have |
| US-6.7 | Platform Super Admin | As a platform super admin, I want to create one-time invoices for setup fees, training, or add-ons so that all revenue is tracked | - POST /api/v1/admin/invoices with type: setup/addon/usage - Custom line items with descriptions and amounts - Not tied to a subscription (subscriptionId nullable) | Should Have |
| US-6.8 | System | As the system, I want to transition subscription status automatically based on payment and dates so that access is managed without manual intervention | - Trial → expired when trialEndsAt passes without payment - Active → past_due when invoice overdue >7 days - past_due → expired when overdue >30 days - Expired companies can only view data, not create new sessions/tickets | Must Have |

---

## Cycle 7 - Department & Team Management

### Cycle Information

| Field | Detail |
|-------|--------|
| **Cycle Name** | Department & Team Management |
| **Cycle ID** | CYC-07 |
| **Goal** | Implement organizational structure within companies: departments that group agents under team leaders, enabling team-based ticket routing, per-department analytics, and management hierarchy |
| **Business Value** | Real call centers have departments (Billing, Technical, Sales). Without this, all agents are in a flat pool — tickets can't be routed to specialists, team leaders can't manage their team specifically, and performance reports are company-wide only. This is essential for companies with 10+ agents |
| **Prerequisites** | Cycle 1 (Company/User), Cycle 5 (Agent workspace — agents now get a departmentId) |
| **Entities Involved** | Department, User (departmentId field) |
| **Depends On** | CYC-01, CYC-05 |
| **Duration** | 1 week |

### User Stories

| ID | Role | Story | Acceptance Criteria | Priority |
|----|------|-------|---------------------|----------|
| US-7.1 | Company Manager | As a company manager, I want to create departments so that I can organize my agents into teams | - CRUD endpoints for /api/v1/admin/departments - Each department: name, description, managerId (optional) - Department scoped to companyId - Names unique per company | Must Have |
| US-7.2 | Company Manager | As a company manager, I want to assign agents and team leaders to departments so that each team has clear membership | - PATCH /api/v1/admin/users/:id with departmentId - User.departmentId references Department - Team leaders manage agents in their department - An agent can only belong to one department at a time | Must Have |
| US-7.3 | Company Manager | As a company manager, I want to assign a team leader as department manager so that they have oversight of their team | - Department.managerId set to a team_leader's userId - Manager can view their department's tickets and agents - Manager receives notifications for their department's activity | Must Have |
| US-7.4 | Team Leader | As a team leader, I want to view only my department's agents and their tickets so that I focus on my team | - GET /api/v1/admin/users?departmentId=X filters by department - GET /api/v1/admin/tickets?departmentId=X shows tickets assigned to department agents - Team leader's view scoped to their department by default | Should Have |
| US-7.5 | System | As the system, I want to support ticket routing by department so that tickets go to the right specialists | - Ticket category can map to a department (e.g. billing tickets → Billing department) - When showing unassigned tickets, agents see tickets relevant to their department first - Company can configure category-to-department mapping | Could Have |

---

## Cycle 8 - Performance Analytics & Quality Assurance

### Cycle Information

| Field | Detail |
|-------|--------|
| **Cycle Name** | Performance Analytics & Quality Assurance |
| **Cycle ID** | CYC-08 |
| **Goal** | Build the agent performance tracking system (daily snapshots, KPI calculation) and the QA review workflow (team leaders score agent conversations). Enable customer satisfaction ratings after ticket resolution |
| **Business Value** | This is Natiq's strongest competitive advantage: "100% conversation review via AI" vs industry's 2-5% manual sampling. Performance tracking addresses P-4 (agent burnout/poor training) by identifying agents who need coaching. CSAT ratings provide the voice of the customer. Directly feeds KPIs: AHT, CSAT, FCR |
| **Prerequisites** | Cycle 4 (Tickets with resolution data), Cycle 5 (Agent system with real activity) |
| **Entities Involved** | AgentPerformance, Rating, QAReview |
| **Depends On** | CYC-04, CYC-05 |
| **Duration** | 2 weeks |

### User Stories

| ID | Role | Story | Acceptance Criteria | Priority |
|----|------|-------|---------------------|----------|
| US-8.1 | System | As the system, I want to calculate daily performance snapshots for each agent so that historical metrics are available instantly | - Daily cron job (or on-demand) calculates per-agent stats - Metrics: ticketsAssigned, ticketsResolved, ticketsClosed, avgHandleTimeMs, avgFirstResponseMs, firstCallResolutionRate, csatAvg, csatCount, messagesHandled - Stored in AgentPerformance with unique (companyId, agentId, period) - Period format: YYYY-MM-DD | Must Have |
| US-8.2 | Customer | As a customer, I want to rate my experience after a ticket is resolved so that I can provide feedback on the service | - After ticket status changes to resolved, customer receives a rating prompt on their channel - POST /api/v1/ratings or via channel message ("Rate 1-5") - Rating stored with: companyId, ticketId, sessionId, customerId, agentId, score (1-5), comment (optional) - One rating per ticket (unique constraint) | Must Have |
| US-8.3 | Team Leader | As a team leader, I want to create QA reviews for agent conversations so that I can score and coach my team | - POST /api/v1/admin/qa-reviews creates a review for a session - Scores: communication (1-10), accuracy (1-10), empathy (1-10), resolution (1-10), compliance (1-10) - overallScore auto-calculated as average - Includes strengths, improvements, and notes text fields - Status: pending → reviewed | Must Have |
| US-8.4 | Agent | As an agent, I want to view my performance metrics so that I know how I'm doing and where to improve | - GET /api/v1/agent/dashboard includes performance summary - Shows: tickets resolved this week/month, avg handle time, CSAT average, latest QA review score - Trend indicators (better/worse than last period) | Must Have |
| US-8.5 | Agent | As an agent, I want to view my QA reviews so that I can read feedback and improve | - GET /api/v1/agent/qa-reviews returns my reviews - Shows scores, strengths, improvements, reviewer name - Agent can dispute a review (status → disputed) with a reason | Should Have |
| US-8.6 | Company Manager | As a company manager, I want to see a leaderboard of agent performance so that I can identify top performers and those needing help | - GET /api/v1/admin/analytics/agents/leaderboard - Ranked by configurable metric (CSAT, FCR, AHT, tickets resolved) - Filterable by department, date range - Shows trend (improving/declining) | Should Have |
| US-8.7 | Company Manager | As a company manager, I want to see company-wide KPIs so that I can track progress toward our targets | - GET /api/v1/admin/analytics/kpis - Returns: avg AHT, avg CSAT, FCR rate, AI resolution rate, total tickets, resolution rate - Compared against targets (from Proposal KPI table) - Filterable by date range and department | Must Have |

---

## Cycle 9 - Sentiment Analysis & AI Intelligence

### Cycle Information

| Field | Detail |
|-------|--------|
| **Cycle Name** | Sentiment Analysis & AI Intelligence |
| **Cycle ID** | CYC-09 |
| **Goal** | Add real-time sentiment analysis to customer conversations and per-company AI configuration. Detect angry/frustrated customers before they escalate, and allow companies to customize AI behavior (provider, language, tone) |
| **Business Value** | Real-time sentiment analysis is a key differentiator (Proposal: "real-time sentiment analysis"). Detects negative sentiment before it becomes a complaint — proactive vs reactive support. Per-company AI config enables each client to have their own AI personality and language preferences, critical for the Arabic-first market |
| **Prerequisites** | Cycle 2 (AI engine and ChatSession), Cycle 8 (performance system to correlate sentiment with agent quality) |
| **Entities Involved** | SentimentLog, AIConfig |
| **Depends On** | CYC-02, CYC-08 |
| **Duration** | 2 weeks |

### User Stories

| ID | Role | Story | Acceptance Criteria | Priority |
|----|------|-------|---------------------|----------|
| US-9.1 | System | As the system, I want to analyze sentiment of each customer message in real-time so that negative trends are detected early | - After each customer message, sentiment analysis runs (async, non-blocking) - SentimentLog created with: sentiment (positive/neutral/negative), score (-1.0 to 1.0), language detected - Stored per message (messageIndex references ChatSession.messages position) - Analysis does not slow down the response to the customer | Must Have |
| US-9.2 | System | As the system, I want to alert when a conversation turns negative so that intervention can happen proactively | - If sentiment score drops below -0.7 (configurable), trigger an alert - Alert emitted via Socket.IO to admin namespace - Alert appears in the manager/leader's dashboard as high-priority - Optional: auto-escalate to human agent if sentiment is very negative | Should Have |
| US-9.3 | Company Manager | As a company manager, I want to configure my company's AI settings so that the AI matches our brand voice and language | - GET/PATCH /api/v1/admin/ai-config for per-company AI configuration - Configurable: provider, model, systemPrompt, temperature, maxTokens, languages[], fallbackBehavior - Default values provided for new companies - Changes take effect immediately on next customer message | Must Have |
| US-9.4 | Company Manager | As a company manager, I want to view sentiment analytics so that I can understand customer mood trends | - GET /api/v1/admin/analytics/sentiment - Returns: sentiment distribution (% positive/neutral/negative), avg score over time, sentiment by channel, sentiment by category - Filterable by date range, channel, agent | Should Have |
| US-9.5 | Company Manager | As a company manager, I want to see which agents improve customer sentiment and which don't so that I can target coaching | - Analytics showing sentiment change after agent joins conversation - Before-agent vs after-agent sentiment comparison - Correlate with agent's CSAT and QA scores | Could Have |

---

## Cycle 10 - Notifications, Tags & Operational Tools

### Cycle Information

| Field | Detail |
|-------|--------|
| **Cycle Name** | Notifications, Tags & Operational Tools |
| **Cycle ID** | CYC-10 |
| **Goal** | Add in-app notifications for staff, flexible tagging for tickets/sessions, canned responses for agent efficiency, and file attachment management. These are operational tools that improve daily workflow efficiency |
| **Business Value** | Notifications ensure nothing falls through the cracks (assigned tickets, QA reviews, urgent messages). Tags enable company-specific classification beyond fixed categories. Canned responses directly reduce AHT. Attachments support evidence-based ticket resolution. Together, these tools increase agent productivity by an estimated 20-30% |
| **Prerequisites** | Cycle 5 (Agent workspace as notification recipient), Cycle 7 (Departments for routing notifications) |
| **Entities Involved** | Notification, Tag, TicketTag, CannedResponse, Attachment |
| **Depends On** | CYC-05, CYC-07 |
| **Duration** | 1 week |

### User Stories

| ID | Role | Story | Acceptance Criteria | Priority |
|----|------|-------|---------------------|----------|
| US-10.1 | Any Staff User | As a staff user, I want to receive in-app notifications so that I'm informed about important events without constantly checking | - Notification created for events: ticket_assigned, ticket_updated, ticket_resolved, new_message, qa_review, performance - Delivered in real-time via Socket.IO (notification:new event) - GET /api/v1/notifications returns my notifications (paginated) - GET /api/v1/notifications/unread-count returns badge number - PATCH /api/v1/notifications/:id/read marks as read | Must Have |
| US-10.2 | Company Manager | As a company manager, I want to create and manage tags so that we can classify tickets with custom labels | - CRUD endpoints for /api/v1/admin/tags - Each tag: name, color (hex), entityType (ticket/session) - Names unique per company per entityType - Tags displayed as colored badges in the UI | Should Have |
| US-10.3 | Agent | As an agent, I want to add tags to tickets so that they can be found and filtered easily | - POST /api/v1/agent/tickets/:ticketId/tags with tagId - Creates TicketTag junction record - Multiple tags per ticket, prevent duplicates - GET /api/v1/agent/tickets supports filtering by tagId | Should Have |
| US-10.4 | Company Manager | As a company manager, I want to create canned responses so that agents can reply faster with pre-approved messages | - CRUD endpoints for /api/v1/admin/canned-responses - Each: title, content, category, shortcut (e.g. /hello) - Shortcut unique per company - Categories: greeting, closing, billing, technical, general | Should Have |
| US-10.5 | Agent | As an agent, I want to use canned responses when replying so that I save time on repetitive answers | - GET /api/v1/agent/canned-responses returns available responses - Filterable by category - Agent selects a response and it's inserted as the reply content - Supports placeholder substitution (e.g. {{customerName}}) | Should Have |
| US-10.6 | Agent | As an agent, I want to attach files to tickets so that I can share screenshots, documents, or evidence | - POST /api/v1/agent/tickets/:ticketId/attachments uploads file via multer - Attachment stored with entityType: ticket, entityId, fileName, fileUrl, mimeType, size - Max file size: 10MB - Allowed types: images, PDF, DOC - Files served at /uploads/ | Should Have |
| US-10.7 | Customer | As a customer, I want to send images/files through chat so that I can show screenshots of my issue | - Telegram: bot receives photos/documents via Bot API - Web: file upload via Socket.IO or REST endpoint - Attachment created with entityType: session - File referenced in ChatSession message meta | Could Have |

---

## Cycle 11 - Admin Dashboard & Reporting

### Cycle Information

| Field | Detail |
|-------|--------|
| **Cycle Name** | Admin Dashboard & Reporting |
| **Cycle ID** | CYC-11 |
| **Goal** | Build comprehensive analytics dashboards and exportable reports for company managers and platform admins. Aggregate all collected data (tickets, sessions, performance, sentiment, QA) into actionable insights |
| **Business Value** | Data-driven decision making is what justifies the Enterprise tier pricing. Managers need to see ROI (cost reduction, CSAT improvement, AHT reduction). The Proposal promises an "18% cost reduction in Year 1" — this dashboard proves it. Premium analytics is also a separate revenue stream (add-on) |
| **Prerequisites** | All previous cycles (data must exist to be reported on) |
| **Entities Involved** | All entities (read-only aggregation) |
| **Depends On** | CYC-01 through CYC-10 |
| **Duration** | 2 weeks |

### User Stories

| ID | Role | Story | Acceptance Criteria | Priority |
|----|------|-------|---------------------|----------|
| US-11.1 | Company Manager | As a company manager, I want a real-time overview dashboard so that I see the current state of my contact center at a glance | - Dashboard shows: active sessions count, open tickets count, online agents count, CSAT today, avg response time today - Data refreshed via Socket.IO (live counters) - Compared to yesterday / last week for trend arrows | Must Have |
| US-11.2 | Company Manager | As a company manager, I want to see ticket analytics so that I understand volume, categories, and resolution patterns | - Charts: tickets by status (pie), tickets over time (line), tickets by category (bar), tickets by channel (donut) - Metrics: total tickets, avg resolution time, resolution rate, backlog (open tickets) - Filterable by date range, category, priority, agent, department | Must Have |
| US-11.3 | Company Manager | As a company manager, I want to see chat/AI analytics so that I understand how well the AI is performing | - Metrics: total sessions, AI resolution rate (no human needed), avg session length, escalation rate - Charts: sessions over time, sessions by channel, AI vs agent resolution, top intents detected - Compares AI performance over time | Must Have |
| US-11.4 | Company Manager | As a company manager, I want to see agent performance reports so that I can manage my workforce effectively | - Per-agent breakdown: tickets handled, avg handle time, CSAT, FCR, QA score - Team/department comparison view - Time-based trends (daily/weekly/monthly) - Exportable as CSV/PDF | Should Have |
| US-11.5 | Company Manager | As a company manager, I want to see customer insights so that I understand who my customers are and what they need | - Top customers by ticket count, repeat customers, customer satisfaction distribution - Most common issues/categories, peak hours for support requests - Channel preference breakdown | Should Have |
| US-11.6 | Platform Super Admin | As a platform super admin, I want a platform-wide dashboard so that I monitor all companies and platform health | - Total companies, total users, total sessions, total tickets (platform-wide) - Revenue: MRR (monthly recurring revenue), active subscriptions by tier, overdue invoices - Per-company breakdown: subscription status, usage vs limits, health score - System metrics: API response times, error rates | Must Have |
| US-11.7 | Company Manager | As a company manager, I want to export reports so that I can share them with stakeholders who don't have platform access | - Export endpoints: GET /api/v1/admin/reports/export?type=tickets&format=csv - Supported formats: CSV, PDF - Supported reports: tickets, agent performance, CSAT, QA reviews - Date range filtering on all exports | Should Have |

---

## Cycle 12 - Testing, Security & Production Launch

### Cycle Information

| Field | Detail |
|-------|--------|
| **Cycle Name** | Testing, Security & Production Launch |
| **Cycle ID** | CYC-12 |
| **Goal** | Comprehensive testing (unit, integration, E2E), security hardening (OWASP top 10 compliance, data encryption, rate limiting), performance optimization, and production deployment preparation |
| **Business Value** | The Proposal mentions "Data quality, security compliance, encrypted logs" as a requirement. Enterprise clients (banks, telecoms) require security certifications before adoption. A production incident on launch would damage reputation irreparably. This cycle ensures Natiq is production-ready and trustworthy |
| **Prerequisites** | All previous cycles completed |
| **Entities Involved** | All entities (tested across all layers) |
| **Depends On** | CYC-01 through CYC-11 |
| **Duration** | 2 weeks |

### User Stories

| ID | Role | Story | Acceptance Criteria | Priority |
|----|------|-------|---------------------|----------|
| US-12.1 | Developer | As a developer, I want unit tests for all services so that I'm confident individual functions work correctly | - Unit tests for all service files (agent.service, chat.service, etc.) - Coverage: >80% line coverage on services - Tests run in isolation with mocked database - CI pipeline runs tests on every push | Must Have |
| US-12.2 | Developer | As a developer, I want integration tests for all API endpoints so that I'm confident the full request-response cycle works | - Integration tests for every route (auth, tickets, chat, admin, agent) - Tests use a test database (not production) - Test RBAC: verify forbidden access for wrong roles - Test validation: verify 400 errors for bad input - Test multi-tenancy: verify cross-company data isolation | Must Have |
| US-12.3 | Developer | As a developer, I want E2E tests for critical flows so that I'm confident the entire system works together | - Test flow: Customer sends message on Telegram → AI responds → Escalation → Ticket created → Agent claims → Agent replies → Customer receives on Telegram → Ticket resolved → Rating submitted - Test flow: Company signup → Subscription created → Agent added → Knowledge base configured → First customer interaction | Must Have |
| US-12.4 | Security | As a security auditor, I want all OWASP top 10 vulnerabilities addressed so that the platform is secure | - SQL/NoSQL injection: all inputs validated via Joi, parameterized queries via Mongoose - XSS: output sanitization, Content-Security-Policy headers - Authentication: JWT with proper expiry, bcrypt password hashing - Authorization: RBAC enforced on every endpoint - Rate limiting: express-rate-limit on auth and public endpoints - CORS: configured for allowed origins only - Helmet.js for security headers | Must Have |
| US-12.5 | System | As the system, I want all sensitive data encrypted at rest and in transit so that customer data is protected | - HTTPS enforced in production (TLS 1.2+) - Database connection uses TLS - Passwords hashed with bcrypt (12 rounds) - JWT tokens have expiry (7 days max) - Environment variables for all secrets (never in code) - Uploaded files scanned for malware | Must Have |
| US-12.6 | DevOps | As a DevOps engineer, I want the application containerized and deployable so that we can run it in production reliably | - Dockerfile for the Node.js application - docker-compose.yml for local development (app + MongoDB + Redis) - Environment configuration via .env files - Health check endpoint: GET /health - Graceful shutdown handling | Must Have |
| US-12.7 | DevOps | As a DevOps engineer, I want monitoring and logging so that I can detect and diagnose production issues | - Structured logging (JSON format) with correlation IDs - Log levels: error, warn, info, debug - Application metrics: response times, error rates, active connections - Database connection monitoring - Alert on: high error rate, slow responses, disk usage | Should Have |
| US-12.8 | System | As the system, I want database indexes optimized so that queries perform well under load | - All indexes defined in models reviewed and tested with explain() - Compound indexes for common query patterns (companyId + status + createdAt) - No full collection scans for paginated endpoints - Target: <100ms response time for all list endpoints | Must Have |

---

## Assumptions & Constraints

### Assumptions

| # | Assumption |
|---|------------|
| A-1 | Companies will have a maximum of 500 agents per account in the first year |
| A-2 | Chat sessions will average 10-20 messages each |
| A-3 | AI provider (Groq) will maintain >99.5% uptime |
| A-4 | Primary market is Egypt and MENA, with Arabic as the primary language |
| A-5 | Customers have access to Telegram or a web browser |
| A-6 | Companies will provide their own Telegram bot tokens |
| A-7 | Internet connectivity is available for all users (no offline mode) |

### Constraints

| # | Constraint |
|---|------------|
| C-1 | Technology stack: Node.js, Express, MongoDB, Socket.IO (already committed) |
| C-2 | Initial deployment on a single server (vertical scaling) before horizontal |
| C-3 | WhatsApp integration requires Facebook Business API approval (mock initially) |
| C-4 | Budget constraints limit to open-source/free-tier AI providers initially (Groq) |
| C-5 | Team size limits parallel development — cycles are sequential |

---

## Risks & Mitigation

| # | Risk | Impact | Probability | Mitigation |
|---|------|--------|-------------|------------|
| R-1 | AI gives incorrect information | High | Medium | RAG ensures answers come from verified knowledge base; confidence threshold triggers escalation |
| R-2 | Groq API rate limits or downtime | High | Low | Fallback behavior configured per company (escalate, default response); support multiple providers |
| R-3 | Data breach / security incident | Critical | Low | OWASP compliance, encryption, RBAC, regular security audits |
| R-4 | Poor Arabic language support from LLM | Medium | Medium | Test extensively with Egyptian dialect; allow companies to customize system prompts; switch to Arabic-optimized models if needed |
| R-5 | Companies exceed plan limits rapidly | Medium | Medium | Clear limit enforcement with friendly upgrade prompts; usage analytics to predict overages |
| R-6 | Agent resistance to AI monitoring | Medium | High | Position as coaching tool not surveillance; show agents their own metrics; highlight improvements |

---

## Glossary

| Term | Definition |
|------|------------|
| **AHT** | Average Handle Time — time from ticket claim to resolution |
| **CSAT** | Customer Satisfaction — rating score (1-5) given by customers |
| **FCR** | First Call Resolution — percentage of issues resolved without reopening |
| **RAG** | Retrieval-Augmented Generation — AI technique using knowledge base for accurate answers |
| **SaaS** | Software as a Service — cloud-hosted subscription model |
| **RBAC** | Role-Based Access Control — permissions determined by user role |
| **Multi-tenant** | Single platform instance serving multiple isolated companies |
| **Escalation** | Transferring a conversation from AI to a human agent |
| **BPO** | Business Process Outsourcing — third-party call center providers |
| **NTRA** | National Telecom Regulatory Authority (Egypt) |
| **KPI** | Key Performance Indicator — measurable success metric |
| **E2E** | End-to-End — testing the full system flow from start to finish |
| **MRR** | Monthly Recurring Revenue — total subscription income per month |
