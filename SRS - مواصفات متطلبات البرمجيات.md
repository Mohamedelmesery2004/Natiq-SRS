# ناتق - مواصفات متطلبات البرمجيات (SRS)

| الحقل | القيمة |
|-------|--------|
| **عنوان المستند** | مواصفات متطلبات البرمجيات (SRS) |
| **اسم المشروع** | ناتق - منصة SaaS لمركز اتصال ذكي متعدد القنوات بالـ AI |
| **الإصدار** | 1.0 |
| **التاريخ** | فبراير 2026 |
| **الحالة** | شغال عليه |
| **السرية** | للاستخدام الداخلي بس |

---

## جدول المحتويات

1. [المقدمة](#1-المقدمة)
2. [نظرة عامة على هيكل النظام](#2-نظرة-عامة-على-هيكل-النظام)
3. [الأدوات والتقنيات](#3-الأدوات-والتقنيات)
4. [ملخص نموذج البيانات](#4-ملخص-نموذج-البيانات)
5. [مواصفات الـ Authentication والـ Authorization](#5-مواصفات-الـ-authentication-والـ-authorization)
6. [معايير تصميم الـ API](#6-معايير-تصميم-الـ-api)
7. [دورة 1 - الأساس والنواة متعددة المستأجرين](#دورة-1---الأساس-والنواة-متعددة-المستأجرين-1)
8. [دورة 2 - محرك الـ AI Chat وقاعدة المعرفة](#دورة-2---محرك-الـ-ai-chat-وقاعدة-المعرفة-1)
9. [دورة 3 - التكامل متعدد القنوات](#دورة-3---التكامل-متعدد-القنوات-1)
10. [دورة 4 - نظام إدارة التذاكر](#دورة-4---نظام-إدارة-التذاكر-1)
11. [دورة 5 - مساحة عمل الـ Agent](#دورة-5---مساحة-عمل-الـ-agent-1)
12. [دورة 6 - الفوترة وإدارة الاشتراكات بتاعة الـ SaaS](#دورة-6---الفوترة-وإدارة-الاشتراكات-بتاعة-الـ-saas-1)
13. [دورة 7 - إدارة الأقسام والفرق](#دورة-7---إدارة-الأقسام-والفرق-1)
14. [دورة 8 - تحليلات الأداء وضمان الجودة](#دورة-8---تحليلات-الأداء-وضمان-الجودة-1)
15. [دورة 9 - تحليل المشاعر وذكاء الـ AI](#دورة-9---تحليل-المشاعر-وذكاء-الـ-ai-1)
16. [دورة 10 - الإشعارات والـ Tags وأدوات التشغيل](#دورة-10---الإشعارات-والـ-tags-وأدوات-التشغيل-1)
17. [دورة 11 - لوحة تحكم الأدمن والتقارير](#دورة-11---لوحة-تحكم-الأدمن-والتقارير-1)
18. [دورة 12 - الاختبارات والأمان وإطلاق الإنتاج](#دورة-12---الاختبارات-والأمان-وإطلاق-الإنتاج-1)
19. [المتطلبات غير الوظيفية](#المتطلبات-غير-الوظيفية)
20. [الواجهات الخارجية](#الواجهات-الخارجية)

---

## 1. المقدمة

### 1.1 الهدف

المستند ده بتاع مواصفات متطلبات البرمجيات (SRS) بيقدم المواصفات التقنية الكاملة لمنصة ناتق. بيوصف المتطلبات الوظيفية (النظام بيعمل إيه)، والمتطلبات غير الوظيفية (بيعمله كويس إزاي)، ونماذج البيانات، وعقود الـ API، والقيود التقنية لكل دورة تطوير.

### 1.2 النطاق

ناتق هي منصة SaaS للـ backend (REST API + WebSocket) مبنية بـ Node.js. المستند ده بيغطي:
- كل الـ 12 دورة تطوير
- 21 كيان في قاعدة البيانات مع تعريفات كاملة للـ schema
- الـ REST API endpoints مع عقود الـ request/response
- مواصفات أحداث الـ Socket.IO
- التكاملات مع أطراف تالتة (Telegram Bot API, Groq LLM API)
- متطلبات الأمان والأداء وقابلية التوسع

### 1.3 التعريفات والاصطلاحات

- **FR**: متطلب وظيفي (Functional Requirement)
- **NFR**: متطلب غير وظيفي (Non-Functional Requirement)
- **Endpoint**: مسار الـ REST API بالصيغة دي `METHOD /path`
- **Socket Event**: حدث Socket.IO بالصيغة دي `event_name`
- **Schema**: هيكل مستند MongoDB/Mongoose
- كل الـ IDs هي MongoDB ObjectId (سلسلة hex من 24 حرف)
- كل الـ timestamps بصيغة ISO 8601 بتوقيت UTC
- كل الـ endpoints بترجع JSON بالهيكل ده: `{ success: boolean, message: string, data: object }`

---

## 2. نظرة عامة على هيكل النظام

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

### 2.1 القرارات المعمارية الرئيسية

| القرار | السبب |
|--------|-------|
| **Multi-tenant بقاعدة بيانات مشتركة** | كل الشركات بتشارك نفس الـ MongoDB instance مع عزل بالـ companyId. أبسط من إن كل شركة يكون ليها database لوحدها، وكفاية للحجم الأولي (أقل من 100 شركة) |
| **Embedded subdocuments للـ Messages والـ AgentNotes** | الرسايل دايماً بتتحمل مع الـ parent بتاعها (ChatSession/Ticket). الـ Embedding بيوفر علينا الـ JOINs وبيحافظ على الترتيب. الـ tradeoff: حد الـ 16MB للمستند (~100 ألف رسالة لكل session — كفاية) |
| **Socket.IO namespaces للعزل** | `/admin` namespace للموظفين، `/webchat` namespace للعملاء. بيمنع العملاء من إنهم يستلموا أحداث الأدمن |
| **JWT للـ authentication اللي مش محتاج state** | مفيش تخزين session على السيرفر. الـ tokens بتشيل companyId والـ role مع كل request. صلاحية 7 أيام بتوازن بين الأمان والراحة |
| **نمط الـ Service layer** | الـ Controllers بتتعامل مع حاجات الـ HTTP؛ الـ Services بتتعامل مع منطق الـ business. الـ Services قابلة لإعادة الاستخدام عبر الـ REST والـ Socket handlers |

---

## 3. الأدوات والتقنيات

| الطبقة | التقنية | الإصدار | الغرض |
|--------|---------|---------|-------|
| Runtime | Node.js | 18+ LTS | تشغيل JavaScript على السيرفر |
| Framework | Express.js | 4.x | سيرفر HTTP والـ routing |
| Real-time | Socket.IO | 4.x | اتصال WebSocket للتحديثات اللحظية |
| Database | MongoDB | 6.x+ | مخزن البيانات الأساسي |
| ODM | Mongoose | 7.x+ | التحقق من الـ Schema، الاستعلامات، الـ indexing |
| Authentication | jsonwebtoken | 9.x | إنشاء والتحقق من الـ JWT |
| Password Hashing | bcrypt | 5.x | تخزين الباسوردات بشكل آمن (12 جولة) |
| Validation | Joi | 17.x | التحقق من الـ request body/param/query |
| File Upload | Multer | 1.x | التعامل مع الـ multipart form-data |
| AI Provider | Groq SDK | latest | الـ LLM API لردود الـ AI |
| HTTP Client | Axios | 1.x | استدعاءات API الخارجية (Telegram, Groq) |
| Cron | node-cron | 3.x | المهام المجدولة (snapshots الأداء اليومية) |
| Security | Helmet | 7.x | HTTP security headers |
| CORS | cors | 2.x | مشاركة الموارد عبر المصادر المختلفة |
| Rate Limiting | express-rate-limit | 7.x | منع إساءة استخدام الـ API |

---

## 4. ملخص نموذج البيانات

تفاصيل الكيانات الكاملة موثقة في `ERD-Documentation.md`. الملخص:

| # | الكيان | اسم الـ Collection | Multi-tenant | العلاقة |
|---|--------|-------------------|-------------|---------|
| 1 | Company | companies | الكيان الجذري | 1:N لكل الكيانات |
| 2 | User | users | أيوا (companyId) | N:1 Company, N:1 Department |
| 3 | ChatSession | chatsessions | أيوا (companyId) | N:1 User, 1:N Message (embedded) |
| 4 | Ticket | tickets | أيوا (companyId) | N:1 User, N:1 User (assignedTo) |
| 5 | KnowledgeItem | knowledgeitems | أيوا (companyId) | N:1 Company |
| 6 | EventLog | eventlogs | أيوا (companyId) | Polymorphic (entityType + entityId) |
| 7 | Plan | plans | لأ (على مستوى المنصة) | 1:N Subscription |
| 8 | Subscription | subscriptions | أيوا (companyId) | N:1 Plan, N:1 Company |
| 9 | Invoice | invoices | أيوا (companyId) | N:1 Subscription, N:1 Company |
| 10 | Department | departments | أيوا (companyId) | N:1 Company, 1:N User |
| 11 | AgentPerformance | agentperformances | أيوا (companyId) | N:1 User |
| 12 | Rating | ratings | أيوا (companyId) | N:1 Ticket, N:1 User |
| 13 | QAReview | qareviews | أيوا (companyId) | N:1 ChatSession, N:1 User |
| 14 | CannedResponse | cannedresponses | أيوا (companyId) | N:1 Company |
| 15 | Tag | tags | أيوا (companyId) | N:N Ticket (عن طريق TicketTag) |
| 16 | TicketTag | tickettags | ضمني | N:1 Ticket, N:1 Tag |
| 17 | Notification | notifications | أيوا (companyId) | N:1 User |
| 18 | Attachment | attachments | أيوا (companyId) | Polymorphic (entityType + entityId) |
| 19 | SentimentLog | sentimentlogs | أيوا (companyId) | N:1 ChatSession |
| 20 | AIConfig | aiconfigs | أيوا (companyId) | 1:1 Company |

---

## 5. مواصفات الـ Authentication والـ Authorization

### 5.1 سير عملية الـ Authentication

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

### 5.2 محتوى الـ JWT Token

```json
{
  "userId": "ObjectId",
  "companyId": "ObjectId",
  "role": "string (enum: ROLES)",
  "iat": "number (issued at)",
  "exp": "number (expires in 7 days)"
}
```

### 5.3 سلسلة الـ Middleware

كل route محمي بيعدي على السلسلة دي بالترتيب:

| # | الـ Middleware | الغرض | رد الفشل |
|---|--------------|-------|----------|
| 1 | `protect` | التحقق من الـ JWT token، تحميل اليوزر من الـ DB، ضبط req.userId, req.companyId, req.user | 401 Unauthorized |
| 2 | `tenantIsolation` | التأكد إن req.companyId متضبط. للـ super_admin اللي عنده body.companyId، بيستخدم ده بدل الـ default | 400 Bad Request |
| 3 | `requirePermission(resource, action)` أو `allowRoles(...roles)` | فحص RBAC_MATRIX[role][resource].includes(action) | 403 Forbidden |
| 4 | `validate(schema)` | تحقق بالـ Joi من req.body, req.params, req.query. stripUnknown: true, abortEarly: false | 400 Bad Request مع التفاصيل |
| 5 | Controller | تنفيذ منطق الـ business | متنوع |

### 5.4 مصفوفة الـ RBAC

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

## 6. معايير تصميم الـ API

### 6.1 هيكل الـ URL

```
Base URL: /api/v1

Admin Routes:     /api/v1/admin/*        (company_manager, team_leader, super_admin)
Agent Routes:     /api/v1/agent/*        (agent role only)
Channel Routes:   /api/v1/channels/*     (public webhooks, customer-facing)
```

### 6.2 صيغة الـ Response

**رد النجاح:**
```json
{
  "success": true,
  "message": "Descriptive success message",
  "data": { ... }
}
```

**رد الخطأ:**
```json
{
  "success": false,
  "message": "Descriptive error message",
  "errors": [ ... ]   // اختياري: مصفوفة أخطاء التحقق
}
```

### 6.3 أكواد حالة الـ HTTP

| الكود | الاستخدام |
|-------|----------|
| 200 | نجاح GET, PATCH, DELETE |
| 201 | نجاح POST (المورد اتعمل) |
| 400 | خطأ في التحقق، طلب غلط (ApiError.badRequest) |
| 401 | الـ JWT token مش موجود أو مش صالح (ApiError.unauthorized) |
| 403 | الـ token صالح بس الصلاحيات مش كفاية (ApiError.forbidden) |
| 404 | المورد مش موجود (ApiError.notFound) |
| 409 | تعارض - مورد مكرر (ApiError.conflict) |
| 500 | خطأ داخلي في السيرفر (ApiError.internal) |

### 6.4 معيار الـ Pagination

كل الـ list endpoints بتدعم:

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

### 6.5 معيار الـ Validation

كل التحقق من الـ requests بيستخدم Joi مع:
```javascript
{
  abortEarly: false,     // بيبلّغ عن كل الأخطاء، مش أول واحدة بس
  stripUnknown: true,    // بيشيل الحقول اللي مش في الـ schema
  allowUnknown: false    // بيرفض الحقول المجهولة
}
```

---

## دورة 1 - الأساس والنواة متعددة المستأجرين

### المتطلبات القبلية
- Node.js 18+ متنصب
- MongoDB 6.x+ شغال
- مفيش اعتمادات على نظام سابق

### الكيانات اللي اتعملت

**Company** — شوف ERD-Documentation.md القسم 4

**User** — شوف ERD-Documentation.md القسم 6

### الـ API Endpoints

#### إدارة الشركات

| Method | Endpoint | Auth | Roles | الوصف |
|--------|----------|------|-------|-------|
| POST | `/api/v1/admin/companies` | JWT | super_admin | إنشاء شركة جديدة |
| GET | `/api/v1/admin/companies` | JWT | super_admin | عرض كل الشركات (paginated) |
| GET | `/api/v1/admin/companies/:id` | JWT | super_admin | جلب شركة بالـ ID |
| PATCH | `/api/v1/admin/companies/:id` | JWT | super_admin | تعديل إعدادات الشركة |
| DELETE | `/api/v1/admin/companies/:id` | JWT | super_admin | تعطيل الشركة (soft delete) |

**POST /api/v1/admin/companies — الطلب:**
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

**الرد (201):**
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

#### إدارة المستخدمين

| Method | Endpoint | Auth | Roles | الوصف |
|--------|----------|------|-------|-------|
| POST | `/api/v1/admin/users` | JWT | super_admin, company_manager, team_leader | إنشاء مستخدم جديد |
| GET | `/api/v1/admin/users` | JWT | super_admin, company_manager, team_leader | عرض المستخدمين (paginated، مع فلاتر) |
| GET | `/api/v1/admin/users/:id` | JWT | super_admin, company_manager, team_leader | جلب مستخدم بالـ ID |
| PATCH | `/api/v1/admin/users/:id` | JWT | super_admin, company_manager | تعديل المستخدم |
| DELETE | `/api/v1/admin/users/:id` | JWT | super_admin, company_manager | تعطيل المستخدم (soft delete) |

#### الـ Authentication

| Method | Endpoint | Auth | Roles | الوصف |
|--------|----------|------|-------|-------|
| POST | `/api/v1/admin/auth/login` | مفيش | super_admin, company_manager, team_leader | تسجيل دخول الموظفين |
| GET | `/api/v1/admin/auth/me` | JWT | أي موظف | جلب بروفايل المستخدم الحالي |

**POST /api/v1/admin/auth/login — الطلب:**
```json
{
  "email": "admin@vodafone.com",
  "password": "securePassword123",
  "companySlug": "vodafone-egypt"
}
```

**الرد (200):**
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

### قواعد التحقق

| الحقل | القواعد |
|-------|---------|
| `name` | مطلوب، string، من 2 لـ 100 حرف، trimmed |
| `email` | مطلوب، صيغة email صالحة، lowercase، trimmed |
| `password` | مطلوب عند الإنشاء، أقل حاجة 6 أحرف |
| `role` | مطلوب، لازم يكون قيمة صالحة من enum الـ ROLES |
| `companySlug` | مطلوب عند تسجيل الدخول، lowercase، trimmed |
| `slug` | مطلوب عند إنشاء الشركة، lowercase، أحرف وأرقام + hyphens، فريد |
| `industry` | اختياري، لازم يكون قيمة enum صالحة |

---

## دورة 2 - محرك الـ AI Chat وقاعدة المعرفة

### المتطلبات القبلية
- دورة 1 خلصت (Company, User, Auth)
- مفتاح Groq API متظبط في الـ environment
- وصول لـ Embedding model API

### الكيانات اللي اتعملت

**ChatSession** — شوف ERD-Documentation.md القسم 7

**Message** (embedded) — شوف ERD-Documentation.md القسم 8

**KnowledgeItem** — شوف ERD-Documentation.md القسم 14

### الـ API Endpoints

#### قاعدة المعرفة

| Method | Endpoint | Auth | Roles | الوصف |
|--------|----------|------|-------|-------|
| POST | `/api/v1/admin/knowledge` | JWT | company_manager, team_leader | إنشاء عنصر معرفة |
| GET | `/api/v1/admin/knowledge` | JWT | أي موظف | عرض عناصر المعرفة (paginated) |
| GET | `/api/v1/admin/knowledge/:id` | JWT | أي موظف | جلب عنصر معرفة بالـ ID |
| PATCH | `/api/v1/admin/knowledge/:id` | JWT | company_manager, team_leader | تعديل عنصر معرفة |
| DELETE | `/api/v1/admin/knowledge/:id` | JWT | company_manager | حذف عنصر معرفة |

#### الـ Embeddings

| Method | Endpoint | Auth | Roles | الوصف |
|--------|----------|------|-------|-------|
| POST | `/api/v1/admin/embeddings/generate` | JWT | company_manager | توليد embeddings لكل عناصر المعرفة النشطة |
| GET | `/api/v1/admin/embeddings/status` | JWT | company_manager | فحص حالة الـ embedding |

#### جلسات الشات (عرض الأدمن)

| Method | Endpoint | Auth | Roles | الوصف |
|--------|----------|------|-------|-------|
| GET | `/api/v1/admin/chat/sessions` | JWT | company_manager, team_leader | عرض الجلسات (paginated، مع فلاتر) |
| GET | `/api/v1/admin/chat/sessions/:id` | JWT | company_manager, team_leader | جلب جلسة مع الرسايل |
| DELETE | `/api/v1/admin/chat/sessions/:id` | JWT | company_manager | إغلاق/حذف جلسة |

### مواصفات خط أنابيب الـ AI

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

الخط ده بيشتغل كده:
1. **تحميل الـ ChatSession**: بندور على جلسة نشطة أو بنعمل واحدة جديدة
2. **فحص لو الـ agent ماسك**: لو `isAgentHandling` بـ true والـ agent مش idle أكتر من 5 دقايق، بنبعت الرسالة للـ agent وبنتخطى الـ AI
3. **فحص كلمات التصعيد**: بندور على كلمات زي agent, human, ticket, complain - لو لقيناها بنعمل ticket
4. **RAG - بحث في قاعدة المعرفة**: بنحسب الـ embedding بتاع الرسالة وبنعمل بحث cosine similarity على KnowledgeItem.embeddingVector - بناخد أعلى 3 عناصر كـ context
5. **استدعاء الـ LLM (Groq)**: بنبعت الـ system prompt + KB context + تاريخ الرسايل + رسالة المستخدم
6. **فحص رد الـ AI للتصعيد**: لو flag الـ shouldEscalate متفعل أو الثقة واطية، بنعمل ticket
7. **حفظ وإرسال**: بنحفظ الرسالة والرد في الـ ChatSession وبنبعت الرد عن طريق القناة (Telegram/Web)

### هيكل طلب الـ LLM

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

## دورة 3 - التكامل متعدد القنوات

### المتطلبات القبلية
- دورة 2 خلصت (ChatSession, AI Pipeline)
- الـ Telegram bot اتعمل عن طريق @BotFather
- Company.channelsConfig.telegram.botToken متظبط

### الكيانات اللي اتعدلت

**User** — اتضاف: `telegramChatId`, `onboardingStep`

### الـ API Endpoints

#### قناة Telegram

| Method | Endpoint | Auth | Roles | الوصف |
|--------|----------|------|-------|-------|
| POST | `/api/v1/channels/telegram/:companySlug/webhook` | Webhook (مفيش JWT) | عام | استقبال تحديثات Telegram |

#### سير معالجة الـ Webhook

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

الـ flow ده بيشتغل كده:
1. **لقي الشركة بالـ slug**: بندور على الشركة من URL params وبنتأكد إن channelsConfig.telegram.isActive
2. **لقي/اعمل يوزر بالـ telegramChatId**: لو يوزر جديد بنعمله بـ role=customer و onboardingStep=1
3. **التعامل مع الأوامر**: `/start` بيقفل الجلسة القديمة ويبعت ترحيب، `/reset` بيقفل الجلسة ويأكد، `/status` بيعرض حالة التذكرة
4. **فحص الـ Onboarding**: الخطوة 1 بنحفظ الاسم ونسأل عن التليفون، الخطوة 2 بنحفظ التليفون ونخلّي الخطوة 0، الخطوة 0 بنكمّل على الـ AI pipeline
5. **خط أنابيب الـ AI**: نفس مواصفات دورة 2، والرد بيتبعت عن طريق Telegram Bot API

#### استدعاءات Telegram Bot API

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

### مواصفات الـ Socket.IO — Web Chat

**Namespace:** `/webchat`

**الاتصال:** `io("wss://domain/webchat", { auth: { sessionId, companyId } })`

| الحدث | الاتجاه | الـ Payload | الوصف |
|-------|---------|-----------|-------|
| `chat:sendMessage` | Client → Server | `{ sessionId, content }` | العميل بيبعت رسالة |
| `chat:message` | Server → Client | `{ sessionId, message: { role, content, timestamp } }` | رد الـ AI أو الـ agent |
| `chat:sessionCreated` | Server → Client | `{ sessionId }` | جلسة جديدة بدأت |
| `chat:sessionClosed` | Server → Client | `{ sessionId }` | الجلسة خلصت |
| `chat:typing` | Server → Client | `{ sessionId }` | الـ AI بيعالج |

---

## دورة 4 - نظام إدارة التذاكر

### المتطلبات القبلية
- دورة 2 خلصت (ChatSession للربط بالسياق)
- دورة 3 خلصت (معلومات القناة لتوصيل الردود)

### الكيانات اللي اتعملت

**Ticket** — شوف ERD-Documentation.md القسم 10

**EventLog** — شوف ERD-Documentation.md القسم 20

### الـ API Endpoints

#### إدارة التذاكر (أدمن)

| Method | Endpoint | Auth | Roles | الوصف |
|--------|----------|------|-------|-------|
| GET | `/api/v1/admin/tickets` | JWT | company_manager, team_leader | عرض كل التذاكر (paginated، مع فلاتر) |
| GET | `/api/v1/admin/tickets/:id` | JWT | company_manager, team_leader | تفاصيل التذكرة |
| PATCH | `/api/v1/admin/tickets/:id` | JWT | company_manager, team_leader | تعديل التذكرة (status, priority, category, assignedTo) |

**GET /api/v1/admin/tickets — الـ Query Parameters:**

| الـ Parameter | النوع | الوصف |
|-------------|-------|-------|
| `page` | Number | رقم الصفحة (افتراضي: 1) |
| `limit` | Number | عدد العناصر في الصفحة (افتراضي: 20، أقصى: 100) |
| `status` | String | فلتر بالحالة (open, in_progress, resolved, closed) |
| `category` | String | فلتر بالفئة |
| `priority` | String | فلتر بالأولوية |
| `assignedTo` | ObjectId | فلتر بالـ agent المعين |
| `channel` | String | فلتر بالقناة المصدر |
| `sort` | String | حقل الترتيب مع بادئة - للتنازلي (افتراضي: -createdAt) |
| `from` | Date | اتعمل بعد التاريخ ده |
| `to` | Date | اتعمل قبل التاريخ ده |

### توليد رقم التذكرة

```
Format: NQ-YYYYMMDD-NNNN

NQ         = Natiq prefix
YYYYMMDD   = Date of creation
NNNN       = Sequential number per company per day (zero-padded)

Example: NQ-20260209-0001 (first ticket for this company on Feb 9, 2026)

Generation: Count existing tickets for (companyId, today) + 1
```

الصيغة بتاعة رقم التذكرة هي `NQ-YYYYMMDD-NNNN` حيث NQ هو بادئة ناتق، YYYYMMDD تاريخ الإنشاء، و NNNN رقم تسلسلي لكل شركة في اليوم (مبطّن بأصفار). مثلاً: `NQ-20260209-0001` (أول تذكرة للشركة دي في 9 فبراير 2026). بيتولّد بعد ما تحسبي التذاكر الموجودة للـ (companyId, النهاردة) + 1.

### ماكينة حالات التذكرة

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

التذكرة بتعدي على الحالات دي:
- **OPEN**: التذكرة اتعملت (عن طريق تصعيد الـ AI أو يدوي)، لسه مفيش agent ماسكها (assignedTo: null)
- **IN_PROGRESS**: الـ agent مسك التذكرة وبيشتغل عليها. ممكن تتنقل لـ agent تاني
- **RESOLVED**: المشكلة اتحلت، مستنيين التأكيد. ممكن تترجع لـ IN_PROGRESS
- **CLOSED**: أرشيف، الإحصائيات اتثبتت. دي حالة نهائية

### تسجيل الأحداث

كل حدث في دورة حياة التذكرة بيعمل EventLog entry:

| الحدث | eventType | metadata |
|-------|-----------|----------|
| التذكرة اتعملت بالـ AI | `ticket_created` | `{ channel, category, intent, confidence, sessionId }` |
| الـ Agent مسك التذكرة | `ticket_claimed` | `{ agentId, agentName }` |
| الـ Agent رد | `agent_replied` | `{ agentId, channel }` |
| التذكرة اتحلت | `ticket_resolved` | `{ agentId, resolutionTimeMs }` |
| التذكرة اتقفلت | `ticket_closed` | `{ closedBy }` |
| التذكرة اتعدلت | `ticket_updated` | `{ changedFields, updatedBy }` |

---

## دورة 5 - مساحة عمل الـ Agent

### المتطلبات القبلية
- دورة 3 خلصت (بنية الـ Socket.IO)
- دورة 4 خلصت (نظام التذاكر)

### الـ API Endpoints

#### تسجيل دخول الـ Agent

| Method | Endpoint | Auth | Roles | الوصف |
|--------|----------|------|-------|-------|
| POST | `/api/v1/agent/auth/login` | مفيش | agent | تسجيل دخول خاص بالـ agent |

#### بروفايل الـ Agent

| Method | Endpoint | Auth | Roles | الوصف |
|--------|----------|------|-------|-------|
| GET | `/api/v1/agent/profile` | JWT | agent | جلب البروفايل بتاعي |
| PATCH | `/api/v1/agent/profile` | JWT | agent | تعديل البروفايل (multipart/form-data) |

**PATCH /api/v1/agent/profile — Multipart Form-Data:**

| الحقل | النوع | الوصف |
|-------|-------|-------|
| `name` | text | اختياري، من 2 لـ 100 حرف |
| `phone` | text | اختياري |
| `password` | text | اختياري، أقل حاجة 6 أحرف (محتاج currentPassword) |
| `currentPassword` | text | مطلوب لو بتغير الباسورد |
| `profileImage` | file | اختياري، jpeg/jpg/png/gif/webp، أقصى حد 5MB |

#### لوحة تحكم الـ Agent

| Method | Endpoint | Auth | Roles | الوصف |
|--------|----------|------|-------|-------|
| GET | `/api/v1/agent/dashboard` | JWT | agent | نظرة عامة على الإحصائيات الشخصية |

**الرد:**
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

#### عمليات التذاكر بتاعة الـ Agent

| Method | Endpoint | Auth | Roles | الوصف |
|--------|----------|------|-------|-------|
| GET | `/api/v1/agent/tickets` | JWT | agent | عرض التذاكر (المعينة ليا + اللي مش متعينة) |
| POST | `/api/v1/agent/tickets/:ticketId/claim` | JWT | agent | مسك تذكرة مفتوحة |
| POST | `/api/v1/agent/tickets/:ticketId/reply` | JWT | agent | الرد على تذكرة (بيتبعت للعميل) |
| PATCH | `/api/v1/agent/tickets/:ticketId/status` | JWT | agent | تعديل حالة التذكرة |
| GET | `/api/v1/agent/tickets/:ticketId/messages` | JWT | agent | جلب رسايل الـ ChatSession المرتبطة |

### مواصفات الـ Socket.IO — الـ Agent اللحظي

**Namespace:** `/admin`

**الاتصال:** `io("wss://domain/admin", { auth: { token: "JWT_TOKEN" } })`

**الـ Authentication:** الـ JWT بيتأكد منه وقت الاتصال. الـ Agent بيدخل الأوضة `company:{companyId}`.

| الحدث | الاتجاه | الـ Payload | الوصف |
|-------|---------|-----------|-------|
| `ticket:watch` | Client → Server | `"ticketId"` | الدخول في أوضة خاصة بالتذكرة للتحديثات اللحظية |
| `ticket:sendMessage` | Client → Server | `{ ticketId, content }` | بعت رسالة للعميل عن طريق الـ agent socket |
| `ticket:messageSent` | Server → Client | `{ ticketId, message }` | تأكيد إن الرسالة اتوصلت |
| `ticket:messageError` | Server → Client | `{ ticketId, error }` | الرسالة فشلت في التوصيل |
| `ticket:message:new` | Server → Client | `{ ticketId, ticketNumber, message }` | رسالة جديدة في تذكرة بتتابعها |
| `ticket:assigned` | Server → Client | `{ ticketId, ticketNumber, agentId, agentName }` | الـ agent مسك التذكرة |
| `ticket:status:changed` | Server → Client | `{ ticketId, status, updatedBy }` | حالة التذكرة اتغيرت |
| `ticket:new` | Server → Client | `{ ticket }` | تذكرة جديدة اتعملت في الشركة |
| `chat:customerMessage` | Server → Client | `{ sessionId, message }` | العميل بعت رسالة (على مستوى الشركة) |

### الـ Agent Idle Fallback

```
Condition: isAgentHandling === true AND
           last agent message timestamp > 5 minutes ago

Trigger:   Next customer message arrives

Action:    1. Message goes through AI pipeline instead of forwarding to agent
           2. isAgentHandling remains true (agent can still respond)
           3. Logged in console: "Agent idle >5min, falling back to AI"

Purpose:   Prevents customers from being stuck waiting for an inactive agent
```

الميكانيزم ده بيشتغل كده: لو الـ `isAgentHandling` بـ true وآخر رسالة من الـ agent كانت من أكتر من 5 دقايق، لما العميل يبعت رسالة جديدة الرسالة بتعدي على خط أنابيب الـ AI بدل ما تتبعت للـ agent. الـ isAgentHandling بيفضل true (يعني الـ agent لسه يقدر يرد). ده عشان العملاء ميفضلوش مستنيين agent مش بيرد.

---

## دورة 6 - الفوترة وإدارة الاشتراكات بتاعة الـ SaaS

### المتطلبات القبلية
- دورة 1 خلصت (كيان Company)

### الكيانات اللي اتعملت

**Plan** — شوف ERD-Documentation.md القسم 1

**Subscription** — شوف ERD-Documentation.md القسم 2

**Invoice** — شوف ERD-Documentation.md القسم 3

### الـ API Endpoints

#### إدارة الخطط

| Method | Endpoint | Auth | Roles | الوصف |
|--------|----------|------|-------|-------|
| POST | `/api/v1/admin/plans` | JWT | super_admin | إنشاء خطة تسعير |
| GET | `/api/v1/admin/plans` | JWT | super_admin | عرض كل الخطط |
| GET | `/api/v1/admin/plans/:id` | JWT | super_admin | تفاصيل الخطة |
| PATCH | `/api/v1/admin/plans/:id` | JWT | super_admin | تعديل الخطة |
| DELETE | `/api/v1/admin/plans/:id` | JWT | super_admin | تعطيل الخطة |

**POST /api/v1/admin/plans — الطلب:**
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

#### إدارة الاشتراكات

| Method | Endpoint | Auth | Roles | الوصف |
|--------|----------|------|-------|-------|
| POST | `/api/v1/admin/subscriptions` | JWT | super_admin | إنشاء اشتراك لشركة |
| GET | `/api/v1/admin/subscriptions` | JWT | super_admin | عرض كل الاشتراكات |
| GET | `/api/v1/admin/subscription` | JWT | company_manager | جلب اشتراك شركتي |
| PATCH | `/api/v1/admin/subscriptions/:id` | JWT | super_admin | تعديل حالة الاشتراك |

#### إدارة الفواتير

| Method | Endpoint | Auth | Roles | الوصف |
|--------|----------|------|-------|-------|
| POST | `/api/v1/admin/invoices` | JWT | super_admin | إنشاء فاتورة (يدوي أو من النظام) |
| GET | `/api/v1/admin/invoices` | JWT | super_admin, company_manager | عرض الفواتير (مفلترة بالشركة للـ managers) |
| PATCH | `/api/v1/admin/invoices/:id` | JWT | super_admin | تعديل حالة الفاتورة (تسجيل الدفع) |

### منطق فرض حدود الخطة

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

الكود ده بيتنفذ في الـ service layer قبل إنشاء أي مورد. بيتأكد إن الشركة عندها اشتراك نشط، وبعدين بيفحص لو المورد المطلوب (agent, session, أو knowledge) وصل لحد الخطة. لو وصل للحد بيرمي ApiError.forbidden.

### ماكينة حالات الاشتراك

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

الاشتراك بيعدي على الحالات دي:
- **TRIAL**: فترة تجريبية 14 يوم. لو اتدفع بينتقل لـ ACTIVE
- **ACTIVE**: الاشتراك شغال. ممكن يتجدد. لو الفاتورة اتأخرت أكتر من 7 أيام بيروح لـ PAST_DUE
- **PAST_DUE**: فترة سماح. لو اتدفع بيرجع ACTIVE. لو التأخير عدّى 30 يوم بيروح لـ EXPIRED
- **EXPIRED**: الاشتراك انتهى (من trial بدون دفع أو تأخير أكتر من 30 يوم)
- **CANCELLED**: المستخدم لغى الاشتراك

---

## دورة 7 - إدارة الأقسام والفرق

### المتطلبات القبلية
- دورة 1 خلصت (Company, User)
- دورة 5 خلصت (مساحة عمل الـ Agent)

### الكيانات اللي اتعملت

**Department** — شوف ERD-Documentation.md القسم 5

### الكيانات اللي اتعدلت

**User** — اتضاف: `departmentId` (ObjectId, ref: Department)

### الـ API Endpoints

| Method | Endpoint | Auth | Roles | الوصف |
|--------|----------|------|-------|-------|
| POST | `/api/v1/admin/departments` | JWT | company_manager | إنشاء قسم |
| GET | `/api/v1/admin/departments` | JWT | company_manager, team_leader | عرض الأقسام |
| GET | `/api/v1/admin/departments/:id` | JWT | company_manager, team_leader | جلب قسم مع الأعضاء |
| PATCH | `/api/v1/admin/departments/:id` | JWT | company_manager | تعديل القسم |
| DELETE | `/api/v1/admin/departments/:id` | JWT | company_manager | تعطيل القسم |

**POST /api/v1/admin/departments — الطلب:**
```json
{
  "name": "Technical Support",
  "description": "Handles network and technical issues",
  "managerId": "ObjectId (team_leader)"
}
```

---

## دورة 8 - تحليلات الأداء وضمان الجودة

### المتطلبات القبلية
- دورة 4 خلصت (التذاكر مع timestamps الحل)
- دورة 5 خلصت (بيانات نشاط الـ Agent)

### الكيانات اللي اتعملت

**AgentPerformance** — شوف ERD-Documentation.md القسم 17

**Rating** — شوف ERD-Documentation.md القسم 18

**QAReview** — شوف ERD-Documentation.md القسم 19

### الـ API Endpoints

#### التقييمات

| Method | Endpoint | Auth | Roles | الوصف |
|--------|----------|------|-------|-------|
| POST | `/api/v1/ratings` | JWT/Channel | customer | إرسال تقييم لتذكرة متحلة |
| GET | `/api/v1/admin/ratings` | JWT | company_manager, team_leader | عرض كل التقييمات |
| GET | `/api/v1/agent/ratings` | JWT | agent | التقييمات اللي استلمتها |

**POST /api/v1/ratings — الطلب:**
```json
{
  "ticketId": "ObjectId",
  "score": 4,
  "comment": "Agent was very helpful, resolved my issue quickly"
}
```

#### مراجعات ضمان الجودة

| Method | Endpoint | Auth | Roles | الوصف |
|--------|----------|------|-------|-------|
| POST | `/api/v1/admin/qa-reviews` | JWT | company_manager, team_leader | إنشاء مراجعة QA |
| GET | `/api/v1/admin/qa-reviews` | JWT | company_manager, team_leader | عرض مراجعات الـ QA (مع فلاتر) |
| PATCH | `/api/v1/admin/qa-reviews/:id` | JWT | company_manager, team_leader | تعديل المراجعة (الدرجات، الحالة) |
| GET | `/api/v1/agent/qa-reviews` | JWT | agent | مراجعات الـ QA بتاعتي |
| PATCH | `/api/v1/agent/qa-reviews/:id/dispute` | JWT | agent | الاعتراض على مراجعة |

**POST /api/v1/admin/qa-reviews — الطلب:**
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

#### تحليلات الأداء

| Method | Endpoint | Auth | Roles | الوصف |
|--------|----------|------|-------|-------|
| GET | `/api/v1/admin/analytics/agents/performance` | JWT | company_manager, team_leader | بيانات أداء الـ agents |
| GET | `/api/v1/admin/analytics/agents/leaderboard` | JWT | company_manager | ترتيب الـ agents |
| GET | `/api/v1/admin/analytics/kpis` | JWT | company_manager | لوحة KPIs بتاعة الشركة |
| GET | `/api/v1/agent/performance` | JWT | agent | مقاييس أدائي |

### الـ Cron بتاع Snapshot الأداء اليومي

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

الـ cron job ده بيشتغل كل يوم الساعة 23:59. بيحسب المقاييس اليومية لكل agent نشط في كل شركة نشطة. بيدور على التذاكر اللي اتعدلت النهاردة وبيحسب المقاييس وبيحفظها في AgentPerformance (بيعمل upsert - يعني بيعدّل لو موجود أو بينشئ لو مش موجود).

---

## دورة 9 - تحليل المشاعر وذكاء الـ AI

### المتطلبات القبلية
- دورة 2 خلصت (محرك الـ AI، ChatSession)
- دورة 8 خلصت (نظام الأداء)

### الكيانات اللي اتعملت

**SentimentLog** — شوف ERD-Documentation.md القسم 16

**AIConfig** — شوف ERD-Documentation.md القسم 15

### الـ API Endpoints

#### إعدادات الـ AI

| Method | Endpoint | Auth | Roles | الوصف |
|--------|----------|------|-------|-------|
| GET | `/api/v1/admin/ai-config` | JWT | company_manager | جلب إعدادات الـ AI بتاعة الشركة |
| PATCH | `/api/v1/admin/ai-config` | JWT | company_manager | تعديل إعدادات الـ AI |

**PATCH /api/v1/admin/ai-config — الطلب:**
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

#### تحليلات المشاعر

| Method | Endpoint | Auth | Roles | الوصف |
|--------|----------|------|-------|-------|
| GET | `/api/v1/admin/analytics/sentiment` | JWT | company_manager | نظرة عامة على المشاعر |
| GET | `/api/v1/admin/analytics/sentiment/sessions/:sessionId` | JWT | company_manager | الجدول الزمني للمشاعر لكل جلسة |

### تكامل تحليل المشاعر

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

التكامل ده بيشتغل كده:
1. الرسالة بتتعالج في خط أنابيب الـ AI الموجود أصلاً
2. بشكل async (مش بيمنع الرد)، الـ Sentiment Analyzer بياخد محتوى الرسالة وبيحلل المشاعر ويطلع sentiment و score و language
3. بنحفظ الـ SentimentLog للتحليلات. لو الـ score أقل من -0.7، بنبعت تنبيه للأدمن وبنفكر في تصعيد تلقائي

---

## دورة 10 - الإشعارات والـ Tags وأدوات التشغيل

### المتطلبات القبلية
- دورة 5 خلصت (مساحة عمل الـ Agent)
- دورة 7 خلصت (الأقسام)

### الكيانات اللي اتعملت

**Notification** — شوف ERD-Documentation.md القسم 21

**Tag** — شوف ERD-Documentation.md القسم 11

**TicketTag** — شوف ERD-Documentation.md القسم 12

**CannedResponse** — شوف ERD-Documentation.md القسم 9

**Attachment** — شوف ERD-Documentation.md القسم 13

### الـ API Endpoints

#### الإشعارات

| Method | Endpoint | Auth | Roles | الوصف |
|--------|----------|------|-------|-------|
| GET | `/api/v1/notifications` | JWT | أي موظف | إشعاراتي (paginated) |
| GET | `/api/v1/notifications/unread-count` | JWT | أي موظف | عدد اللي مش مقروء للـ badge |
| PATCH | `/api/v1/notifications/:id/read` | JWT | أي موظف | تعليم كمقروء |
| PATCH | `/api/v1/notifications/read-all` | JWT | أي موظف | تعليم الكل كمقروء |

#### الـ Tags

| Method | Endpoint | Auth | Roles | الوصف |
|--------|----------|------|-------|-------|
| POST | `/api/v1/admin/tags` | JWT | company_manager | إنشاء tag |
| GET | `/api/v1/admin/tags` | JWT | أي موظف | عرض الـ tags |
| PATCH | `/api/v1/admin/tags/:id` | JWT | company_manager | تعديل tag |
| DELETE | `/api/v1/admin/tags/:id` | JWT | company_manager | حذف tag |
| POST | `/api/v1/agent/tickets/:ticketId/tags` | JWT | agent | إضافة tag لتذكرة |
| DELETE | `/api/v1/agent/tickets/:ticketId/tags/:tagId` | JWT | agent | شيل tag من تذكرة |

#### الردود الجاهزة

| Method | Endpoint | Auth | Roles | الوصف |
|--------|----------|------|-------|-------|
| POST | `/api/v1/admin/canned-responses` | JWT | company_manager | إنشاء رد جاهز |
| GET | `/api/v1/admin/canned-responses` | JWT | أي موظف | عرض الردود الجاهزة |
| PATCH | `/api/v1/admin/canned-responses/:id` | JWT | company_manager | تعديل رد جاهز |
| DELETE | `/api/v1/admin/canned-responses/:id` | JWT | company_manager | حذف رد جاهز |
| GET | `/api/v1/agent/canned-responses` | JWT | agent | عرض الردود (عرض الـ agent) |

#### المرفقات

| Method | Endpoint | Auth | Roles | الوصف |
|--------|----------|------|-------|-------|
| POST | `/api/v1/agent/tickets/:ticketId/attachments` | JWT | agent | رفع ملف على تذكرة |
| GET | `/api/v1/agent/tickets/:ticketId/attachments` | JWT | agent | عرض مرفقات التذكرة |

### أحداث Socket بتاعة الإشعارات

| الحدث | الاتجاه | الـ Payload | المحفز |
|-------|---------|-----------|-------|
| `notification:new` | Server → Client | `{ _id, type, title, message, data, createdAt }` | إشعار جديد اتعمل |
| `notification:count` | Server → Client | `{ unreadCount: number }` | عدد اللي مش مقروء اتحدث |

### محفزات الإشعارات

| الحدث | المستلمين | نوع الإشعار | العنوان |
|-------|----------|-------------|---------|
| تذكرة اتعينت لـ agent | الـ agent المعين | `ticket_assigned` | "New Ticket Assigned" |
| تذكرة اتحلت | العميل (لو عنده حساب موظف)، المدير | `ticket_resolved` | "Ticket Resolved" |
| رسالة جديدة من العميل (agent ماسك) | الـ agent المعين | `new_message` | "New Customer Message" |
| مراجعة QA خلصت | الـ agent اللي اتراجع | `qa_review` | "QA Review Available" |
| snapshot الأداء جاهز | كل agent | `performance` | "Daily Performance Summary" |
| إعلان من النظام | كل الموظفين في الشركة | `system` | قابل للتعديل |

---

## دورة 11 - لوحة تحكم الأدمن والتقارير

### المتطلبات القبلية
- كل الدورات السابقة (لازم يكون فيه بيانات)

### الـ API Endpoints

#### نظرة عامة على لوحة التحكم

| Method | Endpoint | Auth | Roles | الوصف |
|--------|----------|------|-------|-------|
| GET | `/api/v1/admin/dashboard` | JWT | company_manager | نظرة عامة لحظية على الشركة |

**الرد:**
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

#### تحليلات التذاكر

| Method | Endpoint | Auth | Roles | الوصف |
|--------|----------|------|-------|-------|
| GET | `/api/v1/admin/analytics/tickets` | JWT | company_manager | حجم وتوزيع التذاكر |
| GET | `/api/v1/admin/analytics/tickets/trends` | JWT | company_manager | اتجاهات التذاكر مع الوقت |

**GET /api/v1/admin/analytics/tickets — الـ Query Parameters:**

| الـ Parameter | النوع | الوصف |
|-------------|-------|-------|
| `from` | Date | تاريخ البداية |
| `to` | Date | تاريخ النهاية |
| `groupBy` | String | day, week, month |
| `departmentId` | ObjectId | فلتر بالقسم |

**الرد:**
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

#### تحليلات الشات/AI

| Method | Endpoint | Auth | Roles | الوصف |
|--------|----------|------|-------|-------|
| GET | `/api/v1/admin/analytics/chat` | JWT | company_manager | أداء الشات والـ AI |

**الرد:**
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

#### لوحة تحكم أدمن المنصة

| Method | Endpoint | Auth | Roles | الوصف |
|--------|----------|------|-------|-------|
| GET | `/api/v1/admin/platform/dashboard` | JWT | super_admin | إحصائيات على مستوى المنصة كلها |
| GET | `/api/v1/admin/platform/companies` | JWT | super_admin | كل الشركات مع حالة الصحة |

**رد لوحة تحكم المنصة:**
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

#### تصدير التقارير

| Method | Endpoint | Auth | Roles | الوصف |
|--------|----------|------|-------|-------|
| GET | `/api/v1/admin/reports/export` | JWT | company_manager | تصدير تقرير كـ CSV/PDF |

**الـ Query Parameters:**

| الـ Parameter | النوع | الوصف |
|-------------|-------|-------|
| `type` | String | tickets, agents, csat, qa, sessions |
| `format` | String | csv, pdf |
| `from` | Date | تاريخ البداية |
| `to` | Date | تاريخ النهاية |
| `departmentId` | ObjectId | فلتر اختياري |

### أمثلة على MongoDB Aggregation Pipeline

**الـ Aggregation بتاع تحليلات التذاكر:**
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

**الـ Aggregation بتاع ترتيب الـ Agents:**
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

## دورة 12 - الاختبارات والأمان وإطلاق الإنتاج

### المتطلبات القبلية
- كل الدورات السابقة خلصت

### متطلبات الأمان

| # | المتطلب | التنفيذ |
|---|---------|---------|
| SEC-1 | تحقق من المدخلات على كل الـ endpoints | Joi schemas مع stripUnknown، فحص النوع، حدود الطول |
| SEC-2 | منع NoSQL injection | استعلامات Mongoose محمية بالـ parameters، مفيش `$where` أو `$eval` خام |
| SEC-3 | منع XSS | Helmet.js للـ Content-Security-Policy، تنقية المخرجات |
| SEC-4 | Authentication على كل الـ routes المحمية | التحقق من الـ JWT عن طريق middleware الـ `protect` |
| SEC-5 | Authorization (RBAC) | `requirePermission` / `allowRoles` على كل route |
| SEC-6 | عزل بيانات الـ Multi-tenant | فلتر `companyId` على كل استعلام قاعدة بيانات |
| SEC-7 | أمان الباسوردات | bcrypt 12 جولة، أقل حاجة 6 أحرف، مبترجعش أبداً في الردود |
| SEC-8 | Rate limiting | express-rate-limit: 100 طلب/15 دقيقة على الـ auth، 1000 طلب/15 دقيقة على الـ API |
| SEC-9 | إجبار HTTPS | TLS 1.2+ في الإنتاج، HSTS header عن طريق Helmet |
| SEC-10 | إعدادات الـ CORS | قايمة بيضاء بالـ origins المسموحة، مفيش wildcards في الإنتاج |
| SEC-11 | أمان رفع الملفات | تحقق من النوع (صور بس للبروفايلات)، حدود الحجم، أسماء ملفات عشوائية |
| SEC-12 | أسرار البيئة | كل الأسرار في ملف .env، مبتتعملش commit أبداً على git |

### استراتيجية الاختبارات

| المستوى | النطاق | الأداة | هدف التغطية |
|---------|--------|--------|-------------|
| Unit Tests | الـ Services، الأدوات المساعدة، الـ helpers | Jest / Vitest | أكتر من 80% تغطية أسطر |
| Integration Tests | الـ API endpoints (طلب-رد كامل) | Supertest + Jest | كل الـ routes متختبرة |
| E2E Tests | السيناريوهات الحرجة للمستخدمين | Custom scripts | 5 سيناريوهات حرجة |
| Load Tests | الأداء تحت مستخدمين متزامنين | Artillery / k6 | 100 مستخدم متزامن، أقل من 200ms p95 |

### سيناريوهات الاختبار الحرجة E2E

| # | السيناريو | الخطوات |
|---|----------|---------|
| E2E-1 | العميل للـ AI للحل | رسالة Telegram → رد AI → العميل راضي → الجلسة اتقفلت |
| E2E-2 | سيناريو التصعيد الكامل | رسالة عميل → الـ AI مش قادر يحل → تذكرة اتعملت → الـ Agent مسكها → الـ Agent رد (اتوصل على Telegram) → التذكرة اتحلت → التقييم اتبعت |
| E2E-3 | شات الـ Agent اللحظي | الـ Agent بيتصل بالـ socket → بيتابع تذكرة → العميل بيبعت رسالة → الـ Agent بيشوفها فوراً → الـ Agent بيرد عن طريق الـ socket → العميل بيستلمها على Telegram |
| E2E-4 | دورة حياة الـ SaaS | خطة اتعملت → شركة اشتركت (trial) → Agents اتضافوا → جلسات اتعملت → حد الخطة اتفرض → فاتورة اتولدت → الدفع اتسجل |
| E2E-5 | عزل الـ Multi-Tenant | شركتين اتعملوا → Agent من الشركة A بيحاول يوصل لتذكرة الشركة B → 403 Forbidden |

### نشر الإنتاج

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

### الـ Health Check Endpoint

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

## المتطلبات غير الوظيفية

### الأداء

| # | المتطلب | الهدف |
|---|---------|-------|
| NFR-P1 | وقت رد الـ API (الـ list endpoints) | أقل من 200ms (p95) |
| NFR-P2 | وقت رد الـ API (مورد واحد) | أقل من 100ms (p95) |
| NFR-P3 | توصيل رسايل الـ WebSocket | أقل من 50ms (من السيرفر للـ client) |
| NFR-P4 | توليد رد الـ AI | أقل من 3000ms (شامل استدعاء الـ LLM) |
| NFR-P5 | اتصالات WebSocket المتزامنة | 500+ لكل سيرفر |
| NFR-P6 | أداء استعلامات قاعدة البيانات | مفيش full collection scans على الـ paginated endpoints |

### قابلية التوسع

| # | المتطلب | الهدف |
|---|---------|-------|
| NFR-S1 | عدد الشركات المدعومة | 100+ على instance واحد |
| NFR-S2 | عدد المستخدمين لكل شركة | لحد 500 |
| NFR-S3 | جلسات الشات في الشهر (المنصة) | 100,000+ |
| NFR-S4 | تخزين الرسايل لكل جلسة | لحد 500 رسالة |
| NFR-S5 | التوسع الأفقي | تصميم API بدون state، Socket.IO مع Redis adapter لما نحتاج |

### التوفر

| # | المتطلب | الهدف |
|---|---------|-------|
| NFR-A1 | الـ Uptime SLA | 99.5% (أقصى 4.38 ساعة downtime في السنة) |
| NFR-A2 | الإغلاق اللطيف | تكملة الطلبات اللي لسه شغالة قبل التوقف |
| NFR-A3 | استعادة اتصال قاعدة البيانات | إعادة اتصال تلقائية لو الاتصال وقع |
| NFR-A4 | فشل الـ APIs الخارجية (Groq, Telegram) | سلوك بديل، مفيش فشل متسلسل |

### القابلية للصيانة

| # | المتطلب | الهدف |
|---|---------|-------|
| NFR-M1 | هيكل الكود | MVC مع service layer (controller → service → model) |
| NFR-M2 | التعامل مع الأخطاء | مركزي عن طريق asyncHandler و classes الـ ApiError |
| NFR-M3 | الـ Logging | JSON logs مهيكلة مع request correlation IDs |
| NFR-M4 | التوثيق | الـ API موثق عن طريق الـ SRS + Postman collection |

---

## الواجهات الخارجية

### Groq LLM API

| الحقل | القيمة |
|-------|--------|
| Base URL | `https://api.groq.com/openai/v1` |
| Authentication | `Authorization: Bearer GROQ_API_KEY` |
| الـ Endpoint المستخدم | `/chat/completions` |
| الـ Model الافتراضي | `llama-3.3-70b-versatile` |
| Rate Limit | حسب خطة Groq (الخطة المجانية: 30 طلب/دقيقة) |
| الـ Fallback | رجّع رسالة ودية + اعمل ticket لو فشل |

### Telegram Bot API

| الحقل | القيمة |
|-------|--------|
| Base URL | `https://api.telegram.org/bot{token}` |
| Authentication | الـ Bot token في مسار الـ URL |
| الـ Endpoints المستخدمة | `/sendMessage`, `/setWebhook`, `/getWebhookInfo` |
| Webhook | ناتق بيستقبل POST requests من Telegram على `/api/v1/channels/telegram/:slug/webhook` |
| Rate Limit | 30 رسالة/ثانية لنفس الشات، 20 رسالة/دقيقة لنفس الجروب |
| الـ Fallback | إعادة المحاولة بدون Markdown parse_mode لو فشل |

### MongoDB

| الحقل | القيمة |
|-------|--------|
| الاتصال | متغير البيئة `MONGODB_URI` |
| Driver | Mongoose 7.x+ |
| Indexes | معرفة لكل model، compound indexes لأنماط الاستعلام الشائعة |
| Transactions | بتتستخدم للعمليات الذرية لما نحتاج (مسك التذاكر) |
