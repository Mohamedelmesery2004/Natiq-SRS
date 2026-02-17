# Natiq - SRS: UI Design Specification

| Field | Value |
|-------|-------|
| **Document Title** | Software Requirements Specification — UI Design Guide |
| **Project Name** | Natiq - AI-Powered Omnichannel Contact Center SaaS |
| **Version** | 1.0 |
| **Date** | February 2026 |
| **Audience** | UI/UX Designers, Frontend Developers |
| **Purpose** | Define every screen, component, data field, and interaction so the UI designer knows exactly what to build |

---

## Table of Contents

1. [Design System Overview](#1-design-system-overview)
2. [User Roles & Access Map](#2-user-roles--access-map)
3. [Navigation Structure](#3-navigation-structure)
4. [Authentication Screens](#4-authentication-screens)
5. [Platform Super Admin Panel](#5-platform-super-admin-panel)
6. [Company Manager Panel](#6-company-manager-panel)
7. [Team Leader Panel](#7-team-leader-panel)
8. [Agent Workspace](#8-agent-workspace)
9. [Customer Web Chat Widget](#9-customer-web-chat-widget)
10. [Shared Components Library](#10-shared-components-library)
11. [Real-Time Behaviors](#11-real-time-behaviors)
12. [Responsive & Mobile Guidelines](#12-responsive--mobile-guidelines)

---

## 1. Design System Overview

### 1.1 Brand Identity

| Element | Value |
|---------|-------|
| Primary Color | To be defined by designer |
| Language Support | Arabic (RTL) + English (LTR) |
| Default Direction | RTL (Arabic-first market) |
| Font (Arabic) | Cairo, Tajawal, or IBM Plex Arabic |
| Font (English) | Inter, Poppins, or system sans-serif |

### 1.2 Design Principles

1. **Arabic-First**: All layouts must work RTL by default, with LTR as secondary
2. **Data-Dense**: Dashboards show many metrics — use cards, compact tables, and charts
3. **Real-Time Feel**: Live counters, toast notifications, typing indicators — the UI should feel alive
4. **Role-Based**: Each role sees a completely different interface. Never show features the user can't access
5. **Multi-Tenant Aware**: Company branding (name, logo) appears throughout the manager/agent panels

### 1.3 Status Colors (Used Across All Screens)

| Status | Color | Used For |
|--------|-------|----------|
| Open | Blue | Tickets waiting for agent |
| In Progress | Amber/Yellow | Tickets being worked on |
| Resolved | Green | Completed tickets |
| Closed | Gray | Archived tickets |
| Active | Green dot | Online agents, active sessions |
| Inactive | Gray dot | Offline agents, closed sessions |
| Urgent | Red | Urgent priority tickets |
| High | Orange | High priority |
| Medium | Yellow | Medium priority |
| Low | Blue-Gray | Low priority |

### 1.4 Notification Colors

| Type | Color/Icon |
|------|------------|
| ticket_assigned | Blue bell |
| new_message | Green chat bubble |
| ticket_resolved | Green checkmark |
| qa_review | Purple clipboard |
| performance | Orange chart |
| system | Gray info |

---

## 2. User Roles & Access Map

Each role sees a completely different application. The designer must create separate layouts for each:

| Role | Application | Description |
|------|-------------|-------------|
| `platform_super_admin` | Super Admin Panel | Full platform management — companies, plans, subscriptions, invoices, platform analytics |
| `company_manager` | Company Manager Panel | Company-level management — staff, knowledge base, tickets, analytics, settings, billing |
| `team_leader` | Team Leader Panel | Department-focused — team agents, QA reviews, department tickets, team analytics |
| `agent` | Agent Workspace | Ticket-focused — queue, active tickets, real-time chat, personal stats, profile |
| `customer` | Web Chat Widget | Embedded chat widget on company website — conversation with AI/agent |

---

## 3. Navigation Structure

### 3.1 Super Admin — Sidebar Navigation

```
Logo: Natiq Platform

Navigation Items:
├── Dashboard              (platform overview)
├── Companies              (list, create, edit)
│   └── Company Detail     (view company health)
├── Plans                  (pricing tiers CRUD)
├── Subscriptions          (all company subscriptions)
├── Invoices               (all invoices, mark paid)
├── Users                  (platform-wide user search)
└── Settings               (platform config)

Top Bar:
├── Search (global)
├── Notification Bell (with unread count badge)
└── Profile Avatar → Dropdown (Profile, Logout)
```

### 3.2 Company Manager — Sidebar Navigation

```
Logo: Company Name + Logo

Navigation Items:
├── Dashboard              (company overview)
├── Tickets                (all tickets, filters)
├── Chat Sessions          (all conversations)
├── Agents                 (staff management)
│   ├── All Agents
│   ├── Agent Performance
│   └── Leaderboard
├── Departments            (team structure)
├── Knowledge Base         (FAQs, packages, policies)
├── QA Reviews             (quality assurance)
├── Analytics              (charts & reports)
│   ├── Ticket Analytics
│   ├── Chat/AI Analytics
│   ├── Sentiment Analytics
│   └── KPI Dashboard
├── Settings
│   ├── Company Profile
│   ├── Channels Config    (Telegram, Web, WhatsApp)
│   ├── AI Configuration
│   ├── Tags Management
│   └── Canned Responses
├── Billing
│   ├── My Subscription
│   └── Invoices
└── Reports Export

Top Bar:
├── Search
├── Notification Bell (with unread count badge)
└── Profile Avatar → Dropdown (Profile, Settings, Logout)
```

### 3.3 Team Leader — Sidebar Navigation

```
Logo: Company Name

Navigation Items:
├── Dashboard              (department overview)
├── My Department
│   ├── Agents             (department agents only)
│   ├── Tickets            (department tickets)
│   └── Performance        (department metrics)
├── QA Reviews             (create & manage reviews)
├── Knowledge Base         (read + edit)
├── Chat Sessions          (department sessions)
└── Settings               (profile)

Top Bar:
├── Notification Bell
└── Profile Avatar
```

### 3.4 Agent — Sidebar/Top Navigation

```
Logo: Company Name

Navigation Items:
├── Dashboard              (personal stats)
├── Ticket Queue           (unassigned + my tickets)
├── Active Chat            (real-time conversation view)
├── My Performance         (metrics & trends)
├── QA Reviews             (my reviews & feedback)
├── Knowledge Base         (read-only reference)
├── Canned Responses       (quick replies)
└── Profile                (edit profile & image)

Top Bar:
├── Status Toggle (Online/Away/Offline)
├── Notification Bell (with unread count badge)
└── Profile Avatar
```

---

## 4. Authentication Screens

### 4.1 Screen: Admin Login

**URL:** `/admin/login`
**Accessible By:** company_manager, team_leader, platform_super_admin
**Purpose:** Staff login to the admin panel

#### Layout

```
┌─────────────────────────────────────────────────┐
│                                                   │
│              ┌─────────────────────┐              │
│              │     Natiq Logo      │              │
│              │                     │              │
│              │  ┌───────────────┐  │              │
│              │  │ Company Slug  │  │  Text input  │
│              │  └───────────────┘  │              │
│              │  ┌───────────────┐  │              │
│              │  │ Email         │  │  Email input │
│              │  └───────────────┘  │              │
│              │  ┌───────────────┐  │              │
│              │  │ Password      │  │  Password    │
│              │  └───────────────┘  │  (show/hide) │
│              │                     │              │
│              │  [    Login     ]   │  Primary btn │
│              │                     │              │
│              │  Forgot Password?   │  Link        │
│              └─────────────────────┘              │
│                                                   │
│         Background: gradient or illustration       │
└─────────────────────────────────────────────────┘
```

#### Fields

| Field | Type | Validation | Placeholder |
|-------|------|-----------|-------------|
| Company Slug | Text Input | Required, lowercase, no spaces | e.g. "vodafone-egypt" |
| Email | Email Input | Required, valid email format | e.g. "admin@company.com" |
| Password | Password Input | Required, min 6 chars | "Enter your password" |

#### Interactions

| Action | Behavior |
|--------|----------|
| Click "Login" | Send POST /api/v1/admin/auth/login → On success: redirect to Dashboard. On error: show inline error ("Invalid credentials" or "Company not found") |
| Show/hide password | Toggle eye icon to reveal/hide password text |
| Enter key | Submit form |

#### Error States

| Error | Display |
|-------|---------|
| Invalid credentials | Red text below form: "Email or password is incorrect" |
| Company not found | Red text: "Company not found. Check your company slug." |
| Account deactivated | Red text: "Your account has been deactivated. Contact your manager." |
| Network error | Red text: "Connection error. Please try again." |

---

### 4.2 Screen: Agent Login

**URL:** `/agent/login`
**Accessible By:** agent
**Purpose:** Separate login for agents (simpler, focused UI)

#### Layout

Same as Admin Login but with:
- Different heading: "Agent Portal" instead of "Admin Panel"
- Same fields: Company Slug, Email, Password
- Different redirect: → Agent Dashboard

#### API Call
POST `/api/v1/agent/auth/login` (only accepts role: agent)

---

## 5. Platform Super Admin Panel

### 5.1 Screen: Platform Dashboard

**URL:** `/super-admin/dashboard`
**Accessible By:** platform_super_admin only
**Purpose:** Bird's-eye view of the entire Natiq platform

#### Layout

```
┌─────────────────────────────────────────────────────────────┐
│ [Sidebar]  │              Platform Dashboard                 │
│            │                                                  │
│ Dashboard ◀│  ┌──────┐ ┌──────┐ ┌──────┐ ┌──────┐          │
│ Companies  │  │Total │ │Active│ │Total │ │ MRR  │          │
│ Plans      │  │Comp. │ │Comp. │ │Users │ │      │          │
│ Subscript. │  │  12  │ │  10  │ │ 340  │ │45,000│          │
│ Invoices   │  └──────┘ └──────┘ └──────┘ └──────┘          │
│ Users      │                                                  │
│ Settings   │  ┌─────────────────────┐ ┌────────────────────┐ │
│            │  │ Subscriptions by    │ │ Revenue Trend      │ │
│            │  │ Tier (Donut Chart)  │ │ (Line Chart)       │ │
│            │  │                     │ │                    │ │
│            │  │ Free: 2             │ │ Monthly revenue    │ │
│            │  │ Starter: 4          │ │ over last 12 months│ │
│            │  │ Professional: 3     │ │                    │ │
│            │  │ Enterprise: 1       │ │                    │ │
│            │  └─────────────────────┘ └────────────────────┘ │
│            │                                                  │
│            │  ┌─────────────────────────────────────────────┐ │
│            │  │ Companies Health Table                      │ │
│            │  │ Name | Subscription | Agents | Sessions | $ │ │
│            │  │ ─────────────────────────────────────────── │ │
│            │  │ Vodafone | Pro (Active) | 25/25 | 4.2K | OK│ │
│            │  │ Orange   | Starter      | 8/10  | 1.1K | OK│ │
│            │  │ CIB      | Enterprise   | 45/∞  | 8.7K |⚠️ │ │
│            │  └─────────────────────────────────────────────┘ │
│            │                                                  │
│            │  ┌──────────────────┐ ┌────────────────────────┐ │
│            │  │ Overdue Invoices │ │ System Health          │ │
│            │  │ 2 invoices       │ │ API: ✅ <80ms          │ │
│            │  │ Total: 5,998 EGP │ │ DB: ✅ Connected       │ │
│            │  │ [View All]       │ │ Socket: ✅ 142 conn    │ │
│            │  └──────────────────┘ └────────────────────────┘ │
└─────────────────────────────────────────────────────────────┘
```

#### KPI Cards (Top Row)

| Card | Data Source | Display |
|------|------------|---------|
| Total Companies | Count of all companies | Number + trend arrow vs last month |
| Active Companies | Companies with isActive: true + active subscription | Number (green) |
| Total Users | Count of all users platform-wide | Number |
| MRR | Sum of active subscription plan prices | Currency formatted (e.g. "45,000 EGP") |

#### Charts

| Chart | Type | Data |
|-------|------|------|
| Subscriptions by Tier | Donut/Pie | Count per tier (free, starter, professional, enterprise) |
| Revenue Trend | Line Chart | Monthly revenue over last 12 months |

#### Companies Health Table

| Column | Description |
|--------|-------------|
| Company Name | Link to company detail page |
| Subscription | Plan name + status badge (Active/Trial/Expired) |
| Agents | Used/Limit (e.g. "25/25" — show red when at limit) |
| Sessions This Month | Number (show yellow when >80% of limit) |
| Health | Green checkmark (OK), Yellow warning (approaching limits), Red alert (overdue/expired) |

#### Real-Time Updates
- KPI cards update every 30 seconds via Socket.IO
- Overdue invoice count updates on payment events

---

### 5.2 Screen: Companies List

**URL:** `/super-admin/companies`
**Purpose:** View, search, and manage all companies on the platform

#### Layout

```
┌─────────────────────────────────────────────────────────────┐
│ Companies                                    [+ New Company] │
│                                                               │
│ ┌─────────────────────────────────────────────────────────┐  │
│ │ 🔍 Search by name or slug...    [Industry ▼] [Status ▼]│  │
│ └─────────────────────────────────────────────────────────┘  │
│                                                               │
│ ┌───────────────────────────────────────────────────────────┐│
│ │ Name          │ Slug       │ Industry │ Plan    │ Status  ││
│ │───────────────┼────────────┼──────────┼─────────┼─────────││
│ │ Vodafone Egypt│ vodafone-eg│ Telecom  │ Pro     │ 🟢 Active││
│ │ Orange Egypt  │ orange-eg  │ Telecom  │ Starter │ 🟢 Active││
│ │ TestCo        │ testco     │ Other    │ Free    │ 🟡 Trial ││
│ │ OldCompany    │ oldcompany │ Banking  │ -       │ 🔴 Expired││
│ └───────────────────────────────────────────────────────────┘│
│                                                               │
│ Showing 1-10 of 12          [< 1 2 >]                        │
└─────────────────────────────────────────────────────────────┘
```

#### Table Columns

| Column | Data | Sortable | Filterable |
|--------|------|----------|------------|
| Name | company.name | Yes | Search |
| Slug | company.slug | No | Search |
| Industry | company.industry | Yes | Dropdown: telecom, banking, ecommerce, healthcare, other |
| Plan | subscription.plan.name | Yes | Dropdown |
| Status | subscription.status | Yes | Dropdown: active, trial, expired, cancelled |
| Agents | Count of users with role=agent | Yes | - |
| Created | company.createdAt | Yes | Date range |
| Actions | Edit, View, Deactivate | - | - |

#### Actions

| Action | Trigger | Result |
|--------|---------|--------|
| "+ New Company" button | Opens Create Company modal/page | |
| Click company row | Navigate to Company Detail page | |
| Edit icon | Opens Edit Company modal | |
| Deactivate toggle | Confirmation dialog → PATCH company.isActive | |

---

### 5.3 Screen: Create/Edit Company (Modal or Page)

**Purpose:** Add a new company or edit existing one

#### Form Fields

| Section | Field | Type | Required | Notes |
|---------|-------|------|----------|-------|
| **Basic Info** | Company Name | Text | Yes | Max 100 chars |
| | Slug | Text (auto-generated from name) | Yes | Lowercase, hyphens only, unique. Show availability check |
| | Industry | Select Dropdown | Yes | Options: telecom, banking, ecommerce, healthcare, other |
| **Channel Config** | Telegram Bot Token | Text (masked) | No | Show "Test Connection" button |
| | Telegram Active | Toggle Switch | No | Default: off |
| | Web Chat Active | Toggle Switch | No | Default: on |
| | WhatsApp Active | Toggle Switch | No | Default: off (coming soon badge) |
| **AI Settings** | AI Enabled | Toggle Switch | No | Default: on |
| | Escalation Threshold | Slider (0.0 - 1.0) | No | Default: 0.5. Show label: "Lower = more escalations" |
| | Max Session Messages | Number Input | No | Default: 50 |
| **Working Hours** | Start Time | Time Picker | No | Default: 09:00 |
| | End Time | Time Picker | No | Default: 17:00 |
| | Timezone | Select Dropdown | No | Default: Africa/Cairo |

#### Buttons
- **Save** (Primary) → POST or PATCH
- **Cancel** (Secondary) → Close modal

---

### 5.4 Screen: Plans Management

**URL:** `/super-admin/plans`
**Purpose:** CRUD pricing plans

#### Plan Card Layout (Grid View)

```
┌────────────────┐ ┌────────────────┐ ┌────────────────┐ ┌────────────────┐
│   FREE         │ │   STARTER      │ │  PROFESSIONAL  │ │  ENTERPRISE    │
│                │ │                │ │                │ │                │
│   0 EGP/mo    │ │  999 EGP/mo   │ │  2,999 EGP/mo │ │  Custom        │
│                │ │                │ │                │ │                │
│ 3 agents       │ │ 10 agents      │ │ 25 agents      │ │ Unlimited      │
│ 500 sessions   │ │ 2,000 sessions │ │ 5,000 sessions │ │ Unlimited      │
│ 20 KB items    │ │ 100 KB items   │ │ 200 KB items   │ │ Unlimited      │
│                │ │                │ │                │ │                │
│ Channels:      │ │ Channels:      │ │ Channels:      │ │ Channels:      │
│ ✅ Web         │ │ ✅ Web         │ │ ✅ Web         │ │ ✅ All         │
│ ❌ Telegram    │ │ ✅ Telegram    │ │ ✅ Telegram    │ │                │
│ ❌ WhatsApp    │ │ ❌ WhatsApp    │ │ ✅ WhatsApp    │ │ Features:      │
│                │ │                │ │                │ │ ✅ All         │
│ Features:      │ │ Features:      │ │ Features:      │ │ ✅ Custom AI   │
│ ✅ AI Chat     │ │ ✅ AI Chat     │ │ ✅ AI Chat     │ │ ✅ Dedicated   │
│ ❌ Sentiment   │ │ ❌ Sentiment   │ │ ✅ Sentiment   │ │    Support     │
│ ❌ QA Review   │ │ ✅ QA Review   │ │ ✅ QA Review   │ │                │
│ ❌ Analytics+  │ │ ❌ Analytics+  │ │ ✅ Analytics+  │ │                │
│                │ │                │ │                │ │                │
│ [Edit] [⚙️]    │ │ [Edit] [⚙️]    │ │ [Edit] [⚙️]    │ │ [Edit] [⚙️]    │
└────────────────┘ └────────────────┘ └────────────────┘ └────────────────┘

                                                          [+ Create Plan]
```

#### Create/Edit Plan Form

| Field | Type | Required |
|-------|------|----------|
| Name | Text | Yes |
| Tier | Select (free/starter/professional/enterprise) | Yes |
| Price | Number (currency) | Yes |
| Billing Cycle | Radio (Monthly / Annual) | Yes |
| Max Agents | Number | Yes |
| Max Sessions/Month | Number | Yes |
| Max Knowledge Items | Number | Yes |
| Available Channels | Checkboxes (web, telegram, whatsapp) | Yes |
| Features | Checkboxes (ai_chat, sentiment, qa_review, analytics_plus, canned_responses, custom_branding) | Yes |
| AI Enabled | Toggle | Yes |
| Active | Toggle | Yes |

---

### 5.5 Screen: Subscriptions List

**URL:** `/super-admin/subscriptions`

#### Table

| Column | Data |
|--------|------|
| Company | company.name (link to company) |
| Plan | plan.name + tier badge |
| Status | Badge: trial (blue), active (green), past_due (orange), expired (red), cancelled (gray) |
| Start Date | Formatted date |
| End Date / Trial Ends | Formatted date (highlight if ending soon <7 days) |
| Auto Renew | Yes/No toggle |
| Actions | Edit Status, Assign New Plan |

---

### 5.6 Screen: Invoices List

**URL:** `/super-admin/invoices`

#### Table

| Column | Data |
|--------|------|
| Invoice # | INV-YYYY-NNNN (link to detail) |
| Company | company.name |
| Type | Badge: subscription, setup, addon, usage |
| Amount | Currency formatted (e.g. "2,999 EGP") |
| Status | Badge: draft (gray), pending (blue), paid (green), overdue (red), refunded (purple) |
| Due Date | Formatted date. Red text if overdue |
| Paid At | Formatted date or "-" |
| Actions | Mark as Paid, View Details |

#### "Mark as Paid" Action
- Click button → Confirmation dialog: "Mark invoice INV-2026-0042 as paid?"
- On confirm → PATCH status: paid, set paidAt
- Badge changes from pending/overdue to green "Paid"

---

## 6. Company Manager Panel

### 6.1 Screen: Company Dashboard

**URL:** `/manager/dashboard`
**Purpose:** Real-time overview of the company's contact center operations

#### Layout

```
┌─────────────────────────────────────────────────────────────────┐
│ Dashboard                                     Today: Feb 17, 2026│
│                                                                   │
│ ┌────────┐ ┌────────┐ ┌────────┐ ┌────────┐ ┌────────┐         │
│ │Active  │ │Open    │ │Online  │ │CSAT    │ │Avg Resp│         │
│ │Sessions│ │Tickets │ │Agents  │ │Today   │ │Time    │         │
│ │  23    │ │  15    │ │  5/8   │ │ 4.2⭐  │ │ 32sec  │         │
│ │ ↑12%   │ │ ↓3%    │ │        │ │ ↑0.3   │ │ ↓5sec  │         │
│ └────────┘ └────────┘ └────────┘ └────────┘ └────────┘         │
│                                                                   │
│ ┌──────────────────────────────┐ ┌──────────────────────────────┐│
│ │ Tickets Over Time (7 days)  │ │ Tickets by Category          ││
│ │ (Area/Line Chart)           │ │ (Horizontal Bar Chart)       ││
│ │                              │ │                              ││
│ │ Lines: Created, Resolved     │ │ Billing:    ████████ 120     ││
│ │                              │ │ Network:    ██████   95      ││
│ │                              │ │ Complaint:  █████████ 155    ││
│ │                              │ │ Packages:   █████    80      ││
│ └──────────────────────────────┘ └──────────────────────────────┘│
│                                                                   │
│ ┌──────────────────────────────┐ ┌──────────────────────────────┐│
│ │ AI Resolution Rate           │ │ Channel Distribution         ││
│ │ (Gauge/Ring Chart)           │ │ (Donut Chart)                ││
│ │                              │ │                              ││
│ │      ┌────┐                  │ │  Telegram: 65%               ││
│ │      │42% │ of sessions      │ │  Web Chat: 35%               ││
│ │      │    │ resolved by AI   │ │                              ││
│ │      └────┘                  │ │                              ││
│ │ Target: 40% ✅               │ │                              ││
│ └──────────────────────────────┘ └──────────────────────────────┘│
│                                                                   │
│ ┌───────────────────────────────────────────────────────────────┐│
│ │ Recent Tickets                                    [View All →]││
│ │ #NQ-20260217-0003 │ Billing │ 🔴 Urgent │ Omar Agent │ 2min  ││
│ │ #NQ-20260217-0002 │ Network │ 🟡 Medium │ Unassigned │ 15min ││
│ │ #NQ-20260217-0001 │ Complaint│ 🟢 Low   │ Sara Agent │ 1hr   ││
│ └───────────────────────────────────────────────────────────────┘│
└─────────────────────────────────────────────────────────────────┘
```

#### KPI Cards — Data Mapping

| Card | API Field | Live Update | Trend Comparison |
|------|-----------|-------------|-----------------|
| Active Sessions | Count of ChatSession where status=active | Socket.IO: update on session create/close | vs yesterday |
| Open Tickets | Count of Ticket where status=open | Socket.IO: update on ticket events | vs yesterday |
| Online Agents | Count of connected agent sockets | Socket.IO: real-time | Shows X/Total |
| CSAT Today | Average Rating.score for today | Update on new rating | vs yesterday |
| Avg Response Time | Average Ticket.firstResponseAt - Ticket.createdAt for today | Update on first response | vs yesterday |

#### Charts — Data Mapping

| Chart | API Endpoint | Parameters |
|-------|-------------|------------|
| Tickets Over Time | GET /api/v1/admin/analytics/tickets/trends | from, to, groupBy=day |
| Tickets by Category | GET /api/v1/admin/analytics/tickets | from=today-30d |
| AI Resolution Rate | GET /api/v1/admin/analytics/chat | from=today-30d |
| Channel Distribution | GET /api/v1/admin/analytics/chat | from=today-30d |

---

### 6.2 Screen: Tickets List

**URL:** `/manager/tickets`
**Purpose:** View and manage all tickets with powerful filtering

#### Layout

```
┌──────────────────────────────────────────────────────────────────┐
│ Tickets                                                           │
│                                                                    │
│ ┌──────────────────────────────────────────────────────────────┐  │
│ │ Tabs: [All (450)] [Open (15)] [In Progress (8)] [Resolved]  │  │
│ └──────────────────────────────────────────────────────────────┘  │
│                                                                    │
│ Filters Bar:                                                       │
│ [Category ▼] [Priority ▼] [Agent ▼] [Channel ▼] [Date Range 📅]  │
│ [🔍 Search ticket # or customer name...]                          │
│                                                                    │
│ ┌────────────────────────────────────────────────────────────────┐│
│ │ # │ Ticket       │Customer  │Category│Priority│Agent    │Status│Time  ││
│ │───┼──────────────┼──────────┼────────┼────────┼─────────┼──────┼──────││
│ │ 1 │NQ-0217-0003 │M. Mostafa│Billing │🔴Urgent│Omar     │In Prog│2min ││
│ │ 2 │NQ-0217-0002 │A. Hassan │Network │🟡Medium│—        │Open   │15min││
│ │ 3 │NQ-0217-0001 │S. Ali    │Complaint│🟢Low  │Sara     │Resolved│1hr ││
│ │ 4 │NQ-0216-0012 │K. Ibrahim│Packages│🟡Medium│Omar     │Closed │1day ││
│ └────────────────────────────────────────────────────────────────┘│
│                                                                    │
│ Showing 1-20 of 450          [< 1 2 3 ... 23 >]                   │
└──────────────────────────────────────────────────────────────────┘
```

#### Table Columns

| Column | Data | Click Action |
|--------|------|-------------|
| Ticket # | ticket.ticketNumber | Navigate to Ticket Detail |
| Customer | ticket.userId.name | Navigate to Customer Profile |
| Category | Badge with ticket.category | - |
| Priority | Colored dot + text | - |
| Agent | ticket.assignedTo.name or "Unassigned" | Navigate to Agent Profile |
| Status | Colored badge | - |
| Tags | Colored tag badges (if any) | - |
| Time | Time since creation (relative: "2min", "1hr", "3 days") | - |
| Channel | Icon: Telegram/Web | - |

#### Filter Dropdowns

| Filter | Options |
|--------|---------|
| Category | All, Billing, Network, Packages, Complaint, Payment, Refund, Other |
| Priority | All, Urgent, High, Medium, Low |
| Agent | All, Unassigned, [list of company agents] |
| Channel | All, Telegram, Web |
| Date Range | Today, Last 7 days, Last 30 days, Custom range (date picker) |

#### Ticket Row Actions (on hover or click)

| Action | Icon | Description |
|--------|------|-------------|
| View | Eye icon | Open Ticket Detail page |
| Change Priority | Arrow up/down | Dropdown to change priority |
| Reassign | Person icon | Dropdown to select different agent |
| Change Status | Checkmark | Dropdown: open, in_progress, resolved, closed |

---

### 6.3 Screen: Ticket Detail

**URL:** `/manager/tickets/:ticketId`
**Purpose:** Full ticket view with conversation, notes, and actions

#### Layout

```
┌──────────────────────────────────────────────────────────────────┐
│ ← Back to Tickets         Ticket #NQ-20260217-0003               │
│                                                                    │
│ ┌──────────────────────────┐  ┌──────────────────────────────────┐│
│ │ LEFT PANEL (60%)         │  │ RIGHT PANEL (40%)                ││
│ │                          │  │                                  ││
│ │ ┌── Conversation ──────┐ │  │ ┌── Ticket Info ──────────────┐ ││
│ │ │                      │ │  │ │ Status: [In Progress ▼]     │ ││
│ │ │ 👤 Customer:         │ │  │ │ Priority: [🔴 Urgent ▼]     │ ││
│ │ │ "I have a billing    │ │  │ │ Category: [Billing ▼]       │ ││
│ │ │  issue with my plan" │ │  │ │ Channel: 📱 Telegram        │ ││
│ │ │         2:30 PM      │ │  │ │ Created: Feb 17, 2:30 PM    │ ││
│ │ │                      │ │  │ │ First Response: Feb 17, 2:32 │ ││
│ │ │ 🤖 AI:              │ │  │ │ Resolved: —                  │ ││
│ │ │ "I understand you    │ │  │ │                              │ ││
│ │ │  have a billing..."  │ │  │ │ Tags: [VIP] [Recurring] [+] │ ││
│ │ │         2:30 PM      │ │  │ └──────────────────────────────┘ ││
│ │ │                      │ │  │                                  ││
│ │ │ 👤 Customer:         │ │  │ ┌── Customer Info ─────────────┐ ││
│ │ │ "No I need agent"    │ │  │ │ 👤 Mohamed Mostafa           │ ││
│ │ │         2:31 PM      │ │  │ │ 📧 tg_112074@telegram.ph     │ ││
│ │ │                      │ │  │ │ 📱 +20 100 123 4567          │ ││
│ │ │ 🔔 System:           │ │  │ │ 📊 Total tickets: 3          │ ││
│ │ │ "Ticket created,     │ │  │ │ [View Profile →]             │ ││
│ │ │  forwarded to agent" │ │  │ └──────────────────────────────┘ ││
│ │ │         2:31 PM      │ │  │                                  ││
│ │ │                      │ │  │ ┌── Assigned Agent ─────────────┐││
│ │ │ 👨‍💼 Agent Omar:       │ │  │ │ 👨‍💼 Omar Agent                │││
│ │ │ "Hello Mohamed, let  │ │  │ │ 📧 omar@natiq.com            │││
│ │ │  me check your bill" │ │  │ │ [Reassign ▼]                 │││
│ │ │         2:32 PM      │ │  │ └──────────────────────────────┘││
│ │ │                      │ │  │                                  ││
│ │ └──────────────────────┘ │  │ ┌── AI Summary ─────────────────┐││
│ │                          │  │ │ Intent: billing_inquiry        │││
│ │ ┌── Agent Notes ───────┐ │  │ │ Confidence: 0.85              │││
│ │ │ Internal notes only  │ │  │ │ Summary: "Customer disputes   │││
│ │ │ (not visible to      │ │  │ │  charge on Feb statement"     │││
│ │ │  customer)           │ │  │ └──────────────────────────────┘││
│ │ │                      │ │  │                                  ││
│ │ │ [Type a note...]     │ │  │ ┌── Attachments ────────────────┐││
│ │ └──────────────────────┘ │  │ │ 📎 screenshot.png (2.3 MB)   │││
│ │                          │  │ │ 📎 invoice.pdf (1.1 MB)       │││
│ │                          │  │ │ [+ Upload File]               │││
│ │                          │  │ └──────────────────────────────┘││
│ └──────────────────────────┘  └──────────────────────────────────┘│
└──────────────────────────────────────────────────────────────────┘
```

#### Conversation Panel

| Message Type | Visual Style |
|-------------|-------------|
| Customer (user) | Left-aligned bubble, light gray background, user avatar |
| AI (assistant) | Left-aligned bubble, blue-tinted background, robot icon |
| Agent (agent) | Right-aligned bubble, green-tinted background, agent avatar |
| System | Centered text, muted gray, small font, no bubble |

#### Right Panel — Editable Fields

| Field | Edit Type | API Call on Change |
|-------|-----------|-------------------|
| Status | Dropdown | PATCH /api/v1/admin/tickets/:id |
| Priority | Dropdown | PATCH /api/v1/admin/tickets/:id |
| Category | Dropdown | PATCH /api/v1/admin/tickets/:id |
| Assigned Agent | Dropdown (agent list) | PATCH /api/v1/admin/tickets/:id |
| Tags | Click [+] to add from list, click X to remove | POST/DELETE /api/v1/agent/tickets/:id/tags |

---

### 6.4 Screen: Chat Sessions List

**URL:** `/manager/chat-sessions`
**Purpose:** View all customer conversations (AI and agent-handled)

#### Table

| Column | Data |
|--------|------|
| Session ID | chatSession.sessionId (e.g. CHAT-1803-9682) |
| Customer | chatSession.userId.name |
| Channel | Icon + text (Telegram/Web) |
| Status | Badge: active (green), closed (gray) |
| Messages | chatSession.messageCount |
| Agent Handling | Yes (agent name) / No (AI) |
| Linked Ticket | ticket.ticketNumber or "—" |
| Last Activity | Relative time |
| Sentiment | Colored dot: green/yellow/red (if sentiment analysis enabled) |

---

### 6.5 Screen: Staff Management (Agents & Team Leaders)

**URL:** `/manager/agents`
**Purpose:** CRUD for company staff members

#### Layout

```
┌──────────────────────────────────────────────────────────────────┐
│ Staff Management                                [+ Add Staff]     │
│                                                                    │
│ Tabs: [All (18)] [Agents (12)] [Team Leaders (3)] [Managers (1)]  │
│                                                                    │
│ ┌──────────────────────────────────────────────────────────────┐  │
│ │ 🔍 Search...    [Department ▼] [Role ▼] [Status ▼]          │  │
│ └──────────────────────────────────────────────────────────────┘  │
│                                                                    │
│ ┌──────────────────────────────────────────────────────────────┐  │
│ │ [Avatar] Omar Agent     │ Agent    │ Tech Support │ 🟢 Active│  │
│ │          omar@natiq.com │          │              │          │  │
│ │───────────────────────────────────────────────────────────── │  │
│ │ [Avatar] Sara Agent     │ Agent    │ Billing      │ 🟢 Active│  │
│ │          sara@natiq.com │          │              │          │  │
│ │───────────────────────────────────────────────────────────── │  │
│ │ [Avatar] Ahmed Leader   │ T.Leader │ Tech Support │ 🟢 Active│  │
│ │          ahmed@natiq.com│          │              │          │  │
│ └──────────────────────────────────────────────────────────────┘  │
└──────────────────────────────────────────────────────────────────┘
```

#### Add/Edit Staff Form

| Field | Type | Required | Notes |
|-------|------|----------|-------|
| Name | Text | Yes | |
| Email | Email | Yes | Unique per company |
| Password | Password | Yes (create only) | Min 6 chars, show strength indicator |
| Role | Select | Yes | Options: company_manager, team_leader, agent |
| Department | Select | No | List of company departments |
| Phone | Phone | No | |
| Active | Toggle | No | Default: on |

---

### 6.6 Screen: Departments Management

**URL:** `/manager/departments`

#### Layout — Card Grid

```
┌────────────────────┐  ┌────────────────────┐  ┌────────────────────┐
│ Technical Support   │  │ Billing            │  │ Sales              │
│                     │  │                     │  │                     │
│ 👨‍💼 Manager: Ahmed   │  │ 👨‍💼 Manager: —       │  │ 👨‍💼 Manager: Karim   │
│ 👥 Agents: 5        │  │ 👥 Agents: 3        │  │ 👥 Agents: 4        │
│ 🎫 Open Tickets: 8  │  │ 🎫 Open Tickets: 3  │  │ 🎫 Open Tickets: 0  │
│                     │  │                     │  │                     │
│ [Edit] [View Team]  │  │ [Edit] [View Team]  │  │ [Edit] [View Team]  │
└────────────────────┘  └────────────────────┘  └────────────────────┘
                                                  [+ New Department]
```

---

### 6.7 Screen: Knowledge Base

**URL:** `/manager/knowledge-base`
**Purpose:** Manage FAQs, packages, policies that power the AI

#### Layout

```
┌──────────────────────────────────────────────────────────────────┐
│ Knowledge Base                          [+ Add Item] [🔄 Re-embed]│
│                                                                    │
│ Tabs: [All (45)] [Packages (12)] [FAQs (20)] [Policies (8)]      │
│       [Complaint Flows (5)]                                        │
│                                                                    │
│ ┌──────────────────────────────────────────────────────────────┐  │
│ │ 🔍 Search knowledge base...                                  │  │
│ └──────────────────────────────────────────────────────────────┘  │
│                                                                    │
│ ┌──────────────────────────────────────────────────────────────┐  │
│ │ 📦 Premium 100GB Package                    [Package] Active │  │
│ │    Best value for heavy data users                           │  │
│ │    Embedded: ✅ Feb 15, 2026      [Edit] [Deactivate] [Delete]│  │
│ │───────────────────────────────────────────────────────────── │  │
│ │ ❓ How to pay my bill                       [FAQ] Active     │  │
│ │    Payment methods and instructions                          │  │
│ │    Embedded: ✅ Feb 15, 2026      [Edit] [Deactivate] [Delete]│  │
│ │───────────────────────────────────────────────────────────── │  │
│ │ 📋 Refund Policy                            [Policy] Active  │  │
│ │    Terms and conditions for refunds                          │  │
│ │    Embedded: ⚠️ Content changed    [Edit] [Deactivate] [Delete]│ │
│ └──────────────────────────────────────────────────────────────┘  │
└──────────────────────────────────────────────────────────────────┘
```

#### Embedding Status Indicators

| Status | Visual | Meaning |
|--------|--------|---------|
| Embedded | Green check + date | Vector is up-to-date |
| Needs Re-embed | Yellow warning | Content changed after last embedding |
| Not Embedded | Red X | New item, never embedded |

#### "Re-embed All" Button
- Shows confirmation: "This will regenerate embeddings for all items. This may take a few minutes."
- Progress indicator while running
- Updates status indicators on completion

---

### 6.8 Screen: AI Configuration

**URL:** `/manager/settings/ai-config`
**Purpose:** Customize the company's AI behavior

#### Form

| Field | Type | Description for Designer |
|-------|------|------------------------|
| AI Enabled | Toggle (large, prominent) | Master switch — when off, all messages go to queue |
| Provider | Select: Groq, OpenAI, Anthropic | Show model options that change based on provider |
| Model | Select (dynamic based on provider) | e.g. llama-3.3-70b, gpt-4o |
| System Prompt | Large Textarea (10+ lines) | Show character count, syntax highlighting if possible |
| Temperature | Slider (0.0 - 1.0) | Labels: "Precise" on left, "Creative" on right |
| Max Tokens | Number Slider (100 - 2000) | Show approximate response length: "~50 words" to "~500 words" |
| Languages | Multi-select chips | Options: Arabic (ar), English (en), French (fr) |
| Fallback Behavior | Radio buttons | Options: "Escalate to agent", "Retry with different prompt", "Send default message" |

#### Preview Panel
- Show a "Test Prompt" button where manager can type a sample customer message and see the AI response with current settings before saving

---

### 6.9 Screen: Analytics — Ticket Analytics

**URL:** `/manager/analytics/tickets`

#### Filter Bar
- Date Range picker (preset: Today, 7d, 30d, 90d, Custom)
- Department filter
- Category filter
- Priority filter

#### Charts & Metrics

| Chart/Metric | Type | Data |
|-------------|------|------|
| Total Tickets | Big number card | Count with trend |
| Avg Resolution Time | Big number card | e.g. "4.2 hours" with trend arrow |
| Resolution Rate | Big number card | e.g. "95%" |
| Backlog | Big number card | Open ticket count (red if high) |
| Tickets by Status | Pie/Donut chart | open, in_progress, resolved, closed |
| Tickets Over Time | Line/Area chart | Daily created vs resolved |
| Tickets by Category | Horizontal Bar | billing, network, complaint, etc. |
| Tickets by Channel | Donut | telegram, web |
| Tickets by Priority | Stacked Bar | low, medium, high, urgent |
| Top Agents by Resolution | Table | agent name, resolved count, avg time |

---

### 6.10 Screen: Analytics — Sentiment

**URL:** `/manager/analytics/sentiment`

#### Charts

| Chart | Type | Description |
|-------|------|-------------|
| Sentiment Distribution | Donut | % Positive / Neutral / Negative |
| Sentiment Over Time | Area chart (stacked) | Daily sentiment counts |
| Sentiment by Channel | Grouped bar | Telegram vs Web sentiment comparison |
| Avg Sentiment Score | Gauge/Number | -1.0 to 1.0 with color |
| Agent Sentiment Impact | Table | Agent name, avg sentiment before, avg after, delta |
| Most Negative Sessions | Table | Session ID, customer, score, linked ticket |

---

### 6.11 Screen: QA Reviews

**URL:** `/manager/qa-reviews`

#### Table

| Column | Data |
|--------|------|
| Session ID | Link to session detail |
| Agent | Agent name + avatar |
| Reviewer | Reviewer name |
| Overall Score | Colored number (red <5, yellow 5-7, green >7) out of 10 |
| Status | Badge: pending, reviewed, disputed |
| Date | createdAt |
| Actions | View Detail, Edit Scores |

#### Create QA Review — Scoring Form

```
┌──────────────────────────────────────────────────────────────────┐
│ QA Review for Session CHAT-1803-9682                              │
│ Agent: Omar Agent                                                 │
│                                                                    │
│ ┌── Conversation Preview (read-only) ──────────────────────────┐  │
│ │ [Scrollable chat history with all messages]                  │  │
│ └──────────────────────────────────────────────────────────────┘  │
│                                                                    │
│ ┌── Scores ───────────────────────────────────────────────────┐   │
│ │ Communication  [1──────●──────────10]  8                    │   │
│ │ Accuracy       [1────────────●────10]  9                    │   │
│ │ Empathy        [1────●────────────10]  7                    │   │
│ │ Resolution     [1────────────●────10]  9                    │   │
│ │ Compliance     [1──────────────●──10]  10                   │   │
│ │                                                              │   │
│ │ Overall Score: 8.6 / 10                                      │   │
│ └──────────────────────────────────────────────────────────────┘  │
│                                                                    │
│ Strengths:     [________________________________________________] │
│ Improvements:  [________________________________________________] │
│ Notes:         [________________________________________________] │
│                                                                    │
│                              [Cancel]  [Submit Review]             │
└──────────────────────────────────────────────────────────────────┘
```

---

### 6.12 Screen: Settings — Tags Management

**URL:** `/manager/settings/tags`

#### Layout

```
┌──────────────────────────────────────────────────────┐
│ Tags                                    [+ New Tag]   │
│                                                        │
│ Tabs: [Ticket Tags] [Session Tags]                     │
│                                                        │
│ ┌────────────────────────────────────────────────────┐│
│ │ 🔴 VIP              │ ticket │ [Edit] [Delete]     ││
│ │ 🟡 Recurring Issue   │ ticket │ [Edit] [Delete]     ││
│ │ 🔵 Needs Follow-up   │ ticket │ [Edit] [Delete]     ││
│ │ 🟢 Quick Resolution  │ ticket │ [Edit] [Delete]     ││
│ │ 🟣 Training Example  │ session│ [Edit] [Delete]     ││
│ └────────────────────────────────────────────────────┘│
└──────────────────────────────────────────────────────┘
```

#### Create Tag Form (Small Modal)

| Field | Type |
|-------|------|
| Name | Text (max 30 chars) |
| Color | Color Picker (preset palette + custom hex) |
| Entity Type | Radio: Ticket / Session |

---

### 6.13 Screen: Settings — Canned Responses

**URL:** `/manager/settings/canned-responses`

#### Table

| Column | Data |
|--------|------|
| Title | e.g. "Greeting - Arabic" |
| Shortcut | e.g. "/hello" (monospace font) |
| Category | Badge: greeting, closing, billing, technical, general |
| Content Preview | First 50 chars of content |
| Actions | Edit, Delete |

#### Create/Edit Form

| Field | Type | Notes |
|-------|------|-------|
| Title | Text | Descriptive name |
| Shortcut | Text (prefixed with /) | Auto-adds / prefix, unique per company |
| Category | Select | greeting, closing, billing, technical, general |
| Content | Large Textarea | Show placeholder support: "Use {{customerName}} for customer's name" |

---

### 6.14 Screen: Billing — My Subscription

**URL:** `/manager/billing/subscription`

#### Layout

```
┌──────────────────────────────────────────────────────────────────┐
│ My Subscription                                                    │
│                                                                    │
│ ┌──────────────────────────────────────────────────────────────┐  │
│ │ Current Plan: PROFESSIONAL                                   │  │
│ │ Price: 2,999 EGP / month                                     │  │
│ │ Status: 🟢 Active                                            │  │
│ │ Renews: March 1, 2026                                        │  │
│ │ Auto-Renew: ✅ On                                             │  │
│ └──────────────────────────────────────────────────────────────┘  │
│                                                                    │
│ ┌── Usage ──────────────────────────────────────────────────────┐ │
│ │ Agents:           ████████████████░░░░  20 / 25 (80%)        │ │
│ │ Sessions (month): ██████████░░░░░░░░░░  2,400 / 5,000 (48%) │ │
│ │ Knowledge Items:  ████░░░░░░░░░░░░░░░░  45 / 200 (23%)      │ │
│ └──────────────────────────────────────────────────────────────┘  │
│                                                                    │
│ Features Included:                                                 │
│ ✅ AI Chat  ✅ Telegram  ✅ Web Chat  ✅ Sentiment Analysis        │
│ ✅ QA Review  ✅ Advanced Analytics  ❌ WhatsApp  ❌ Custom Branding│
│                                                                    │
│ [Contact Sales to Upgrade]                                         │
└──────────────────────────────────────────────────────────────────┘
```

#### Usage Bars
- Green: <70% usage
- Yellow: 70-90% usage
- Red: >90% usage
- Show "Limit Reached" badge when at 100%

---

## 7. Team Leader Panel

### 7.1 Screen: Department Dashboard

**Same layout as Manager Dashboard** (Section 6.1) but filtered to the team leader's department only:
- KPI cards show department metrics only
- Charts show department data
- Recent tickets show only department tickets

### 7.2 Screen: Department Agents

**Same layout as Staff Management** (Section 6.5) but:
- Only shows agents in the team leader's department
- Cannot add agents from other departments
- Shows quick performance stats per agent (resolved today, CSAT)

### 7.3 Screen: QA Reviews

**Same as Section 6.11** — Team leader is the primary reviewer

---

## 8. Agent Workspace

### 8.1 Screen: Agent Dashboard

**URL:** `/agent/dashboard`
**Purpose:** Personal performance overview and quick access to work

#### Layout

```
┌──────────────────────────────────────────────────────────────────┐
│ Good morning, Omar 👋                         [🟢 Online ▼]       │
│                                                                    │
│ ┌────────┐ ┌────────┐ ┌────────┐ ┌────────┐                      │
│ │My      │ │Resolved│ │Avg Resp│ │My CSAT │                      │
│ │Tickets │ │Today   │ │Time    │ │        │                      │
│ │   5    │ │   3    │ │ 45sec  │ │ 4.5⭐  │                      │
│ └────────┘ └────────┘ └────────┘ └────────┘                      │
│                                                                    │
│ ┌── My Active Tickets ─────────────────────────────────────────┐  │
│ │ 🔴 #NQ-0217-0003 │ Billing │ M. Mostafa │ 2min ago  [Open →]│  │
│ │ 🟡 #NQ-0217-0001 │ Network │ A. Hassan  │ 15min ago [Open →]│  │
│ │ 🟡 #NQ-0216-0010 │ Packages│ S. Ali     │ 1hr ago   [Open →]│  │
│ └──────────────────────────────────────────────────────────────┘  │
│                                                                    │
│ ┌── Unassigned Queue ──────────────────────────────────────────┐  │
│ │ 🔴 #NQ-0217-0005 │ Complaint │ Urgent │ Telegram │ [Claim]  │  │
│ │ 🟡 #NQ-0217-0004 │ Billing   │ Medium │ Web      │ [Claim]  │  │
│ │ 🟢 #NQ-0217-0002 │ Packages  │ Low    │ Telegram │ [Claim]  │  │
│ │                                                               │  │
│ │ 12 tickets in queue                                           │  │
│ └──────────────────────────────────────────────────────────────┘  │
│                                                                    │
│ ┌── Weekly Performance ────────────────────────────────────────┐  │
│ │ (Mini bar chart: tickets resolved per day this week)         │  │
│ │ Mon: ██ 2                                                     │  │
│ │ Tue: ████ 4                                                   │  │
│ │ Wed: ███ 3                                                    │  │
│ │ Thu: █████ 5 (today)                                          │  │
│ └───────────────────────���──────────────────────────────────────┘  │
└──────────────────────────────────────────────────────────────────┘
```

#### Agent Status Toggle (Top Right)

| Status | Color | Meaning |
|--------|-------|---------|
| Online | Green | Available for new tickets, receiving real-time messages |
| Away | Yellow | Temporarily unavailable (break, meeting) |
| Offline | Gray | Not accepting new work |

#### "Claim" Button Behavior
1. Click "Claim" → Instant loading spinner on button
2. API: POST /api/v1/agent/tickets/:ticketId/claim (atomic operation)
3. On success: Ticket moves from "Unassigned Queue" to "My Active Tickets" with animation
4. On fail (already claimed): Toast notification "This ticket was claimed by another agent"
5. Customer notified on their channel automatically

---

### 8.2 Screen: Active Chat (The Core Agent Screen)

**URL:** `/agent/tickets/:ticketId/chat`
**Purpose:** Real-time conversation with the customer — this is where agents spend 90% of their time

#### Layout

```
┌──────────────────────────────────────────────────────────────────────┐
│ ┌── Ticket List    ┐ ┌── Chat Area (60%)──────────┐ ┌── Info (25%)─┐│
│ │ (15% sidebar)    │ │                             │ │              ││
│ │                  │ │ #NQ-0217-0003 │ M.Mostafa   │ │ Customer     ││
│ │ My Tickets:      │ │ 📱 Telegram   │ 🔴 Urgent   │ │ Mohamed M.   ││
│ │                  │ │                             │ │ Telegram     ││
│ │ ● NQ-0003 🔴     │ │ ┌─────────────────────────┐│ │ +20 100...   ││
│ │   M. Mostafa     │ │ │ 👤 10:30 AM             ││ │              ││
│ │   "billing..."   │ │ │ I have a billing issue   ││ │ ──────────── ││
│ │   2min ago       │ │ │ with my February bill    ││ │ Ticket Info  ││
│ │                  │ │ └─────────────────────────┘│ │ Status: IP   ││
│ │ ● NQ-0001 🟡     │ │                             │ │ Priority: 🔴  ││
│ │   A. Hassan      │ │ ┌─────────────────────────┐│ │ Category:    ││
│ │   "network..."   │ │ │ 🤖 10:30 AM             ││ │  Billing     ││
│ │   15min ago      │ │ │ I see you have a billing ││ │ Created:     ││
│ │                  │ │ │ concern. Let me connect  ││ │  2:30 PM     ││
│ │ ● NQ-0010 🟡     │ │ │ you with an agent.      ││ │              ││
│ │   S. Ali         │ │ └─────────────────────────┘│ │ ──────────── ││
│ │   "packages..."  │ │                             │ │ Quick Actions││
│ │   1hr ago        │ │ ┌─────────────────────────┐│ │ [Resolve ✅]  ││
│ │                  │ │ │ 🔔 10:31 AM             ││ │ [Close 🔒]    ││
│ │ ── Unassigned ── │ │ │ Agent Omar has joined    ││ │ [Transfer ↗] ││
│ │                  │ │ └─────────────────────────┘│ │              ││
│ │ ○ NQ-0005 🔴     │ │                             │ │ ──────────── ││
│ │   K. Ibrahim     │ │ ┌─────────────────────────┐│ │ Canned       ││
│ │   [Claim]        │ │ │       👨‍💼 10:32 AM        ││ │ Responses    ││
│ │                  │ │ │ Hello Mohamed! I'm Omar, ││ │ /hello       ││
│ │                  │ │ │ let me check your bill   ││ │ /billing     ││
│ │                  │ │ └─────────────────────────┘│ │ /thanks      ││
│ │                  │ │                             │ │ /escalate    ││
│ │                  │ │                             │ │              ││
│ │                  │ │ ┌─────────────────────────┐│ │ ──────────── ││
│ │                  │ │ │ 📎 │ Type message... [→] ││ │ Knowledge    ││
│ │                  │ │ │    │                     ││ │ Base         ││
│ │                  │ │ └─────────────────────────┘│ │ [Search KB]  ││
│ └──────────────────┘ └─────────────────────────────┘ └──────────────┘│
└──────────────────────────────────────────────────────────────────────┘
```

#### Left Sidebar — Ticket List

| Element | Description |
|---------|-------------|
| My Tickets section | Agent's assigned tickets, sorted by last activity |
| Each ticket card | Ticket number, priority dot, customer name, last message preview, time |
| Active ticket | Highlighted background |
| Unread messages | Bold text + unread count badge |
| Unassigned section | Open tickets available to claim |
| Claim button | Inline claim, no page navigation needed |

#### Center — Chat Area

| Element | Description |
|---------|-------------|
| Header | Ticket number, customer name, channel icon, priority badge |
| Messages | Chronological, auto-scroll to bottom, lazy-load older messages on scroll up |
| Message bubbles | Different colors per role (see Section 6.3) |
| Timestamps | Grouped by day ("Today", "Yesterday", "Feb 15"), individual time on each message |
| Typing indicator | "Customer is typing..." animation when customer is typing (web channel only) |
| Input bar | Text area (expandable), Attach file button (📎), Send button (→) |
| Canned response trigger | When agent types "/" → dropdown of matching canned responses |

#### Right Panel — Info & Actions

| Section | Content |
|---------|---------|
| Customer Info | Name, email, phone, channel, total tickets count |
| Ticket Info | Status, priority, category, created date, first response time |
| Quick Actions | Resolve button (green), Close button (gray), Transfer button |
| Canned Responses | List of shortcuts, click to insert into message input |
| Knowledge Base | Search box to find relevant articles, click to preview |

#### Real-Time Behavior (Socket.IO)

| Event | UI Response |
|-------|-------------|
| Customer sends message | New message bubble appears with animation, notification sound |
| Agent sends message | Message appears immediately (optimistic update), then confirmed |
| Send fails | Message shows red X with "Failed to send. [Retry]" |
| Another agent claims a ticket from queue | Ticket disappears from "Unassigned" with fade animation |
| Ticket resolved by another agent | Status updates in sidebar |
| New ticket created (company-wide) | Appears in "Unassigned" section with subtle animation |

#### Canned Response Autocomplete

```
Agent types: /bi

┌────────────────────────────────┐
│ /billing-check                  │  "Let me check your billing..."
│ /billing-resolved               │  "Your billing issue has been..."
│ /billing-escalate               │  "I'll escalate this to our..."
└────────────────────────────────┘

Agent selects → content inserted into message input
Agent can edit before sending
```

---

### 8.3 Screen: My Performance

**URL:** `/agent/performance`

#### Layout

```
┌──────────────────────────────────────────────────────────────────┐
│ My Performance                          Period: [This Month ▼]    │
│                                                                    │
│ ┌────────┐ ┌────────┐ ┌────────┐ ┌────────┐ ┌────────┐          │
│ │Resolved│ │Avg Time│ │CSAT    │ │FCR     │ │QA Score│          │
│ │  42    │ │ 28min  │ │ 4.5⭐  │ │ 88%    │ │ 8.2/10 │          │
│ │ ↑12%   │ │ ↓15%   │ │ ↑0.3   │ │ ↑5%    │ │ ↑0.5   │          │
│ └────────┘ └────────┘ └────────┘ └────────┘ └────────┘          │
│                                                                    │
│ ┌── Tickets Resolved Over Time ────────────────────────────────┐  │
│ │ (Bar chart: daily resolved count for selected period)        │  │
│ └──────────────────────────────────────────────────────────────┘  │
│                                                                    │
│ ┌── Recent Ratings ────────────────────────────────────────────┐  │
│ │ ⭐⭐⭐⭐⭐ "Great service!" — M. Mostafa — #NQ-0217-0003      │  │
│ │ ⭐⭐⭐⭐   "Good but slow"  — A. Hassan  — #NQ-0216-0010      │  │
│ │ ⭐⭐⭐⭐⭐ "Very helpful"    — S. Ali     — #NQ-0216-0008      │  │
│ └──────────────────────────────────────────────────────────────┘  │
│                                                                    │
│ ┌── Latest QA Review ──────────────────────────────────────────┐  │
│ │ Reviewed by: Ahmed Leader │ Feb 15, 2026                     │  │
│ │ Communication: 8 │ Accuracy: 9 │ Empathy: 7 │ Score: 8.2    │  │
│ │ Strengths: "Excellent technical knowledge"                   │  │
│ │ Improve: "Show more empathy with frustrated customers"       │  │
│ │ [View All Reviews →]                                         │  │
│ └──────────────────────────────────────────────────────────────┘  │
└──────────────────────────────────────────────────────────────────┘
```

---

### 8.4 Screen: Agent Profile

**URL:** `/agent/profile`

#### Form (Multipart/Form-Data)

```
┌──────────────────────────────────────────────────────────────────┐
│ My Profile                                                        │
│                                                                    │
│ ┌────────────────────────────────────────────────────────────────┐│
│ │         ┌───────────┐                                          ││
│ │         │           │  ← Click to upload new photo             ││
│ │         │  [Photo]  │     Supported: JPG, PNG, GIF, WebP       ││
│ │         │           │     Max size: 5MB                         ││
│ │         └───────────┘                                          ││
│ │                                                                ││
│ │ Name:            [Omar Agent          ]                        ││
│ │ Email:           omar@natiq.com (read-only, grayed out)        ││
│ │ Phone:           [+20 100 123 4567    ]                        ││
│ │ Department:      Technical Support (read-only)                  ││
│ │ Role:            Agent (read-only)                              ││
│ │                                                                ││
│ │ ── Change Password ──────────────────                          ││
│ │ Current Password: [                   ]                        ││
│ │ New Password:     [                   ]  Strength: ████ Strong ││
│ │ Confirm Password: [                   ]                        ││
│ │                                                                ││
│ │                                    [Cancel]  [Save Changes]    ││
│ └────────────────────────────────────────────────────────────────┘│
└──────────────────────────────────────────────────────────────────┘
```

---

## 9. Customer Web Chat Widget

### 9.1 Widget: Chat Bubble & Window

**Purpose:** Embedded on the company's website — customers click to chat with AI or agent

#### Collapsed State (Bubble)

```
                              ┌──────┐
                              │ 💬   │  ← Floating bottom-right
                              │      │     Pulsing animation on first visit
                              └──────┘     Badge shows unread count
```

#### Expanded State (Chat Window)

```
                    ┌──────────────────────────────┐
                    │ Company Name ✕               │  Header (company color)
                    │ 🟢 Online                     │
                    ├──────────────────────────────┤
                    │                              │
                    │ 🤖 Welcome! How can I help?   │  AI welcome message
                    │                              │
                    │            I have a billing  │  User message (right)
                    │            question          │
                    │                              │
                    │ 🤖 Sure! I can help with     │  AI response (left)
                    │    billing. What specific    │
                    │    issue do you have?        │
                    │                              │
                    │ ···                          │  Typing indicator
                    │                              │
                    ├──────────────────────────────┤
                    │ 📎 │ Type a message...  [→]  │  Input bar
                    └──────────────────────────────┘

    Widget size: ~370px wide × 500px tall
    Position: Fixed bottom-right, 20px margin
    Responsive: Full-screen on mobile
```

#### Chat Messages

| Sender | Style |
|--------|-------|
| AI (assistant) | Left aligned, company primary color background, robot avatar |
| Agent (agent) | Left aligned, slightly different shade, agent name + avatar shown |
| Customer (user) | Right aligned, gray background |
| System | Centered, small text, muted color ("Agent Omar has joined") |

#### States

| State | Display |
|-------|---------|
| AI handling | Robot avatar, "AI Assistant" label |
| Agent handling | Agent avatar + name, "Agent Omar" label |
| Typing (AI) | Three dots animation |
| Offline / Outside hours | "We're currently offline. Leave a message and we'll get back to you." |
| Session ended | "This conversation has ended. [Start New Chat]" |

#### Rating Prompt (After Resolution)

```
┌──────────────────────────────┐
│ How was your experience?      │
│                                │
│    ⭐ ⭐ ⭐ ⭐ ⭐               │  Click to rate 1-5
│                                │
│ [Add a comment (optional)]     │  Text area
│                                │
│ [Submit]  [Skip]               │
└──────────────────────────────┘
```

---

## 10. Shared Components Library

These components are reused across all panels:

### 10.1 Notification Center

```
Click bell icon →
┌──────────────────────────────────┐
│ Notifications            [Mark all read] │
│                                          │
│ 🔵 New ticket assigned                   │
│    #NQ-0217-0005 assigned to you         │
│    2 minutes ago                          │
│                                          │
│ 🟢 Ticket resolved                       │
│    #NQ-0217-0001 resolved by Sara        │
│    15 minutes ago                         │
│                                          │
│ 🟣 QA Review available                   │
│    Ahmed reviewed your session            │
│    1 hour ago                             │
│                                          │
│ [View All Notifications →]               │
└──────────────────────────────────────────┘
```

- Unread: Bold text + blue dot
- Read: Normal text, no dot
- Click: Navigate to relevant entity (ticket, review, etc.)
- Real-time: New notifications appear at top with slide animation + sound

### 10.2 Data Table Component

Standard table used everywhere:
- Sortable columns (click header to sort, arrow indicator)
- Pagination (page numbers + prev/next)
- Row hover highlight
- Row click navigates to detail
- Loading skeleton while fetching
- Empty state: illustration + "No [items] found" message

### 10.3 Filter Bar Component

Standard filter bar:
- Search input (debounced, 300ms)
- Dropdown filters (single select)
- Date range picker
- "Clear All Filters" button
- Active filter count badge

### 10.4 Status Badge Component

| Status | Background | Text Color |
|--------|-----------|------------|
| open | Blue-100 | Blue-800 |
| in_progress | Amber-100 | Amber-800 |
| resolved | Green-100 | Green-800 |
| closed | Gray-100 | Gray-800 |
| active | Green-100 | Green-800 |
| trial | Blue-100 | Blue-800 |
| expired | Red-100 | Red-800 |
| paid | Green-100 | Green-800 |
| overdue | Red-100 | Red-800 |
| urgent | Red-100 | Red-800 |

### 10.5 Toast/Snackbar Notifications

- Success: Green, auto-dismiss 3 seconds
- Error: Red, manual dismiss required
- Warning: Yellow, auto-dismiss 5 seconds
- Info: Blue, auto-dismiss 3 seconds
- Position: Bottom-center or top-right

### 10.6 Confirmation Dialog

Used for destructive actions (deactivate, delete, status change):
```
┌────────────────────────────────────┐
│ ⚠️ Confirm Action                   │
│                                      │
│ Are you sure you want to deactivate  │
│ agent "Omar Agent"?                  │
│                                      │
│ This will prevent them from logging  │
│ in and receiving new tickets.        │
│                                      │
│              [Cancel]  [Deactivate]  │
└────────────────────────────────────┘
```

### 10.7 Empty States

Every list/table should have an empty state:

| Screen | Empty State Message | Illustration |
|--------|-------------------|--------------|
| Tickets | "No tickets yet. They'll appear here when customers need help." | Support desk illustration |
| Chat Sessions | "No conversations yet. Share your chat widget link to get started." | Chat bubbles illustration |
| Knowledge Base | "No knowledge items. Add FAQs and policies to train your AI." | Books/brain illustration |
| Notifications | "All caught up! No new notifications." | Checkmark illustration |
| Agent Queue | "No unassigned tickets. Great job team!" | High-five illustration |

---

## 11. Real-Time Behaviors

### 11.1 Events That Update the UI Instantly (via Socket.IO)

| Event | Affected Screens | UI Update |
|-------|-----------------|-----------|
| New customer message | Agent Chat, Manager Sessions | New bubble appears, notification sound, unread badge |
| Agent sends message | Agent Chat (confirmation), Customer Widget | Message confirmed (checkmark), customer sees message |
| Ticket created | Manager Dashboard, Agent Queue | Counter increments, ticket appears in list |
| Ticket claimed | Agent Queue (both agents) | Ticket removed from queue with animation |
| Ticket resolved | Manager Dashboard, Agent Dashboard | Counter updates, status badge changes |
| New notification | All panels (bell icon) | Badge count increments, dropdown shows new item |
| Agent comes online | Manager Dashboard | Online agents count updates |
| Negative sentiment alert | Manager Dashboard | Red alert card appears |

### 11.2 Optimistic Updates

For better UX, update the UI immediately before server confirmation:

| Action | Optimistic Behavior | On Failure |
|--------|-------------------|------------|
| Send message | Show message in chat immediately with pending icon | Show red X + "Failed to send" |
| Claim ticket | Move ticket to "My Tickets" immediately | Move back + toast "Already claimed" |
| Mark read | Remove bold/badge immediately | Revert on error |

---

## 12. Responsive & Mobile Guidelines

### 12.1 Breakpoints

| Breakpoint | Width | Layout Changes |
|-----------|-------|---------------|
| Desktop | > 1280px | Full sidebar + content + info panel |
| Tablet | 768-1280px | Collapsible sidebar, content full width, info panel as drawer |
| Mobile | < 768px | Bottom navigation, full-screen views, no sidebar |

### 12.2 Agent Chat — Mobile

- Ticket list → Full screen list view
- Click ticket → Full screen chat view (back button to list)
- Info panel → Swipe from right or toggle button
- Canned responses → Bottom sheet

### 12.3 Customer Widget — Mobile

- Full screen (not floating window)
- Input bar fixed at bottom
- Messages take full width
- Back button to close chat

### 12.4 RTL (Right-to-Left) Considerations

- All layouts mirror horizontally for Arabic
- Sidebar appears on the right
- Text alignment: right for Arabic content
- Icons that indicate direction (arrows) must flip
- Numbers remain LTR even in RTL layout
- Chat bubbles: customer on left, agent on right (flipped from LTR)
