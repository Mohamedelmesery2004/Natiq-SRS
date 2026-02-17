# Natiq - SRS: Screen Views Specification

| Field | Value |
|-------|-------|
| **Document Title** | Screen-by-Screen View Specification |
| **Project Name** | Natiq - AI-Powered Omnichannel Contact Center SaaS |
| **Version** | 1.0 |
| **Date** | February 2026 |
| **Audience** | UI/UX Designers |
| **Purpose** | Define exactly what every screen shows in every state — every card, field, label, number, chart, table cell, button, and empty state |
| **Companion Doc** | SRS - UI Design Specification.md (wireframes, navigation, design system) |

---

## How to Read This Document

Each screen specification follows this format:

1. **View Title & URL** — Where it lives in the app
2. **Who Sees It** — Which role(s) can access this view
3. **Page Header** — Title, subtitle, action buttons visible at top
4. **View Sections** — Each visual section on the page, top to bottom, left to right
5. **Data Fields** — Every piece of data shown, with label, data source, format, and example
6. **States** — What the view looks like when: loading, empty, populated, error
7. **Actions** — Every clickable element and what it does
8. **Navigation** — Where the user came from and where they can go

---

## Table of Contents

1. [Authentication Views](#1-authentication-views)
2. [Platform Super Admin Views](#2-platform-super-admin-views)
3. [Company Manager Views](#3-company-manager-views)
4. [Team Leader Views](#4-team-leader-views)
5. [Agent Workspace Views](#5-agent-workspace-views)
6. [Customer Web Chat Widget Views](#6-customer-web-chat-widget-views)
7. [Shared Views & Overlays](#7-shared-views--overlays)

---

# 1. Authentication Views

## 1.1 Admin Login View

**URL:** `/admin/login`
**Who Sees It:** Unauthenticated users (redirects to dashboard if already logged in)

### Page Content

| # | Element | What It Shows | Example |
|---|---------|--------------|---------|
| 1 | Page background | Full-screen gradient or brand illustration | Light-to-dark blue gradient with abstract network illustration |
| 2 | Centered card | White card (max 420px wide) containing the login form | Shadow elevation, rounded corners |
| 3 | Logo | Natiq platform logo at top of card | SVG logo, 120px wide |
| 4 | Heading | "Admin Panel" text | Font size 24px, bold, center-aligned |
| 5 | Subtitle | "Sign in to manage your company" | Font size 14px, muted gray |

### Form Fields

| # | Label | Input Type | Placeholder | Validation Rule | Error Message |
|---|-------|-----------|-------------|-----------------|---------------|
| 1 | Company Slug | Text input with icon (building) | "company-slug" | Required, lowercase, no spaces, min 2 chars | "Enter your company slug" / "Slug must be lowercase without spaces" |
| 2 | Email | Email input with icon (envelope) | "admin@company.com" | Required, valid email format | "Enter a valid email address" |
| 3 | Password | Password input with icon (lock) + show/hide toggle | "Enter your password" | Required, min 6 chars | "Password must be at least 6 characters" |

### Buttons & Links

| # | Element | Label | Position | Style |
|---|---------|-------|----------|-------|
| 1 | Submit button | "Sign In" | Full width below form | Primary color, large, bold text |
| 2 | Forgot password link | "Forgot your password?" | Below submit button, center | Text link, muted color |
| 3 | Agent login link | "Agent? Login here" | Below forgot password | Text link, smaller font |

### View States

| State | What the User Sees |
|-------|-------------------|
| **Default** | Empty form, submit button enabled, no errors |
| **Typing** | Field border changes to primary color on focus. Real-time validation runs on blur |
| **Submitting** | Button shows spinner, text changes to "Signing in...", all fields disabled |
| **Error - invalid credentials** | Red banner above form: "Invalid email or password. Please try again." Password field cleared, email stays |
| **Error - company not found** | Red banner: "Company not found. Check your company slug and try again." Company slug field highlighted red |
| **Error - account deactivated** | Red banner: "Your account has been deactivated. Please contact your company administrator." |
| **Error - network** | Red banner: "Unable to connect to server. Check your internet connection." Submit button shows "Retry" |
| **Success** | Brief green checkmark animation, then redirect to role-appropriate dashboard |

### After Successful Login — Redirect Rules

| User Role | Redirects To |
|-----------|-------------|
| platform_super_admin | `/super-admin/dashboard` |
| company_manager | `/manager/dashboard` |
| team_leader | `/team-leader/dashboard` |

---

## 1.2 Agent Login View

**URL:** `/agent/login`
**Who Sees It:** Unauthenticated agents

### Differences from Admin Login

| Element | Change |
|---------|--------|
| Heading | "Agent Portal" instead of "Admin Panel" |
| Subtitle | "Sign in to your workspace" |
| Background | Same gradient but different accent color or illustration showing headset/support |
| API endpoint | POST `/api/v1/agent/auth/login` |
| Redirect on success | `/agent/dashboard` |
| Admin link | "Manager? Login here" → links to `/admin/login` |

### Additional Validation

| Scenario | What Happens |
|----------|-------------|
| User tries to login but their role is not `agent` | Error: "This login is for agents only. If you're a manager or team leader, use the Admin login." |

---

# 2. Platform Super Admin Views

## 2.1 Platform Dashboard View

**URL:** `/super-admin/dashboard`
**Who Sees It:** `platform_super_admin` only
**Purpose:** Bird's-eye view of the entire Natiq SaaS platform health

### Page Header

| Element | Content |
|---------|---------|
| Title | "Platform Dashboard" |
| Subtitle | "Overview of all companies and platform metrics" |
| Right side | Today's date: "Monday, February 17, 2026" |

### Section A: KPI Cards Row (4 cards, equal width)

#### Card 1: Total Companies

| Element | What It Shows | Example |
|---------|--------------|---------|
| Icon | Building icon (top-left of card) | Blue icon |
| Label | "Total Companies" | Gray, small text |
| Value | Count of ALL companies in the platform | "12" — large bold number |
| Trend | Percentage change vs previous month | "↑ 8% vs last month" — green text if positive, red if negative |
| Sub-value | New this month | "+2 this month" — muted small text |

#### Card 2: Active Companies

| Element | What It Shows | Example |
|---------|--------------|---------|
| Icon | Green check-circle icon | |
| Label | "Active Companies" | |
| Value | Companies where `isActive: true` AND subscription `status` in (`active`, `trial`) | "10" |
| Trend | vs previous month | "↑ 2 vs last month" |
| Sub-value | Companies in trial | "3 in trial" — blue badge |

#### Card 3: Total Users

| Element | What It Shows | Example |
|---------|--------------|---------|
| Icon | Users/people icon | |
| Label | "Total Users" | |
| Value | Count of ALL users across all companies (excluding customers) | "340" |
| Breakdown | By role, small text below | "12 managers, 28 leaders, 300 agents" |

#### Card 4: Monthly Recurring Revenue (MRR)

| Element | What It Shows | Example |
|---------|--------------|---------|
| Icon | Currency/money icon | |
| Label | "MRR" | |
| Value | Sum of `plan.price` for all active subscriptions | "45,000 EGP" — formatted with commas |
| Trend | vs previous month | "↑ 12%" — green |
| Sub-value | Annual projection | "~540,000 EGP/year" |

### Section B: Charts Row (2 charts, side by side)

#### Chart 1: Subscriptions by Plan Tier (Left, 50% width)

| Element | What It Shows |
|---------|--------------|
| Type | Donut chart |
| Segments | One segment per tier: Free (gray), Starter (blue), Professional (purple), Enterprise (gold) |
| Center text | Total count: "12 Companies" |
| Legend | Below chart — tier name + count + percentage: "Free: 2 (17%)", "Starter: 4 (33%)", "Professional: 5 (42%)", "Enterprise: 1 (8%)" |
| Hover | Hovering a segment shows tooltip: "Professional — 5 companies — 42%" |

#### Chart 2: Revenue Trend (Right, 50% width)

| Element | What It Shows |
|---------|--------------|
| Type | Line chart with area fill |
| X-axis | Last 12 months: "Mar", "Apr", ..., "Feb" |
| Y-axis | Revenue in EGP (auto-scaled) |
| Line | Monthly total revenue (sum of paid invoices) |
| Dots | Each month has a data point |
| Hover | Tooltip: "January 2026: 42,500 EGP" |
| Target line | Dashed horizontal line if revenue target exists |

### Section C: Companies Health Table (Full width)

#### Table Header

| Element | Content |
|---------|---------|
| Section title | "Companies Health" |
| Right action | "View All →" link — navigates to `/super-admin/companies` |

#### Table Columns — What Each Cell Shows

| Column | Cell Content | Example | Format |
|--------|-------------|---------|--------|
| Company Name | `company.name` — clickable link | "Vodafone Egypt" | Blue text, bold, click → company detail |
| Slug | `company.slug` | "vodafone-eg" | Monospace font, muted gray |
| Subscription | `plan.name` + status badge | "Professional" + green "Active" badge | Plan name in regular text, status as colored badge |
| Agents | `usedAgents / plan.maxAgents` | "25 / 25" | Red text when at limit, orange when >80%, green otherwise |
| Sessions This Month | Count of `ChatSession` created this month | "4,200" | Number with comma separator |
| Session Limit | `plan.maxSessionsPerMonth` | "/ 5,000" | Muted text after sessions count |
| Health | Health indicator icon | Green checkmark / Yellow warning / Red alert | Green = all good, Yellow = approaching limits (>80%), Red = overdue invoice or expired subscription |

#### Table Shows

| Rule | Detail |
|------|--------|
| Rows shown | Top 10 companies sorted by sessions this month (descending) |
| Pagination | None — this is a summary view. "View All" goes to full list |
| Sorting | Click column headers to sort |
| Empty state | "No companies registered yet. Create your first company." with [+ Create Company] button |

### Section D: Bottom Row (2 cards, side by side)

#### Card 1: Overdue Invoices (Left, 50% width)

| Element | What It Shows | Example |
|---------|--------------|---------|
| Title | "Overdue Invoices" | Red accent color |
| Count | Number of invoices where `status: 'overdue'` | "2 overdue invoices" |
| Total amount | Sum of overdue invoice amounts | "Total: 5,998 EGP" |
| List | Top 3 overdue invoices: invoice number + company + amount | "INV-2026-0038 — Vodafone Egypt — 2,999 EGP" |
| Action | "View All Invoices →" | Links to `/super-admin/invoices?status=overdue` |
| Empty state | "No overdue invoices" with green checkmark icon |

#### Card 2: System Health (Right, 50% width)

| Element | What It Shows | Example |
|---------|--------------|---------|
| Title | "System Health" | |
| API Status | Green/red dot + response time | "API: ● 78ms" — green dot |
| Database | Connection status | "Database: ● Connected" — green dot |
| Socket.IO | Active connections count | "WebSocket: ● 142 connections" — green dot |
| Last updated | Timestamp | "Updated 30 seconds ago" |
| Auto-refresh | Every 30 seconds | Subtle pulse animation on refresh |

---

## 2.2 Companies List View

**URL:** `/super-admin/companies`
**Purpose:** Full list of all companies with CRUD actions

### Page Header

| Element | Content |
|---------|---------|
| Title | "Companies" |
| Subtitle | "Manage all companies on the platform" |
| Action button | "+ New Company" — primary button, top-right |

### Filter Bar

| Filter | Type | Options | Default |
|--------|------|---------|---------|
| Search | Text input with search icon | Searches `name` and `slug` | Empty |
| Industry | Dropdown | All, Telecom, Banking, E-Commerce, Healthcare, Other | All |
| Status | Dropdown | All, Active, Trial, Expired, Cancelled | All |
| Sort by | Dropdown | Name (A-Z), Name (Z-A), Newest, Oldest, Most Agents, Most Sessions | Newest |

### Table — Every Column Explained

| Column | Shows | Format | Sortable | Click Action |
|--------|-------|--------|----------|-------------|
| # | Row number | Sequential number (page-relative: 1, 2, 3...) | No | — |
| Company Name | `company.name` | Bold text | Yes | Navigate to company detail `/super-admin/companies/:id` |
| Slug | `company.slug` | Monospace, muted | No | — |
| Industry | `company.industry` | Capitalized badge | Yes | — |
| Plan | Current subscription's `plan.name` or "No plan" | Text with tier color: Free=gray, Starter=blue, Pro=purple, Ent=gold | Yes | — |
| Sub. Status | `subscription.status` | Colored badge: active=green, trial=blue, expired=red, cancelled=gray, past_due=orange | Yes | — |
| Agents | Count of `User` with `role: 'agent'` in this company | Number, e.g. "12" | Yes | — |
| Created | `company.createdAt` | "Feb 15, 2026" | Yes | — |
| Actions | Action icons | 3 icons in a row | No | See below |

### Row Actions

| Icon | Label | Click Action |
|------|-------|-------------|
| Eye icon | View | Navigate to `/super-admin/companies/:id` (detail page) |
| Pencil icon | Edit | Open edit company modal |
| Toggle switch | Activate/Deactivate | If active: confirmation dialog "Deactivate Vodafone Egypt? All their users will lose access." → PATCH `isActive: false`. If inactive: "Reactivate?" → PATCH `isActive: true` |

### Pagination

| Element | Shows |
|---------|-------|
| Left text | "Showing 1-10 of 12 companies" |
| Right controls | Page numbers: [< 1 2 >] |
| Per-page selector | "Show: 10 | 25 | 50" |

### View States

| State | What the User Sees |
|-------|-------------------|
| **Loading** | Table skeleton: 5 rows of gray animated placeholder bars |
| **Empty (no companies)** | Illustration of buildings + "No companies registered yet. Get started by creating your first company." + [+ Create Company] button |
| **Empty (no filter results)** | "No companies match your filters." + [Clear Filters] button |
| **Populated** | Full table with data |

---

## 2.3 Company Detail View

**URL:** `/super-admin/companies/:companyId`
**Purpose:** Deep view into a single company's health and configuration

### Page Header

| Element | Content |
|---------|---------|
| Back link | "← Back to Companies" |
| Title | Company name: "Vodafone Egypt" |
| Status badge | Active (green) or Inactive (red) |
| Action buttons | [Edit Company] [Deactivate] |

### Section A: Company Info Card

| Field | Label | Shows | Example |
|-------|-------|-------|---------|
| 1 | Company Name | `company.name` | "Vodafone Egypt" |
| 2 | Slug | `company.slug` | "vodafone-eg" — monospace |
| 3 | Industry | `company.industry` | "Telecom" — badge |
| 4 | Created | `company.createdAt` | "January 5, 2026" |
| 5 | Channels | List of active channels | "Telegram ●" "Web Chat ●" "WhatsApp ○" (green dot = active, gray = inactive) |
| 6 | AI Enabled | `company.settings.aiEnabled` | Green "Enabled" or Red "Disabled" |
| 7 | Working Hours | `company.settings.workingHours` | "09:00 - 17:00 (Africa/Cairo)" |

### Section B: Subscription Details Card

| Field | Shows | Example |
|-------|-------|---------|
| Current Plan | `plan.name` + `plan.tier` | "Professional (professional)" |
| Price | `plan.price` + `plan.billingCycle` | "2,999 EGP / month" |
| Status | `subscription.status` | Green "Active" badge |
| Start Date | `subscription.startDate` | "January 1, 2026" |
| End Date | `subscription.endDate` | "January 31, 2027" |
| Trial Ends | `subscription.trialEndsAt` or "—" | "—" (not in trial) |
| Auto Renew | `subscription.autoRenew` | "Yes" with green toggle |

### Section C: Usage Meters (3 progress bars)

| Meter | Current | Limit | Percentage | Color Rule |
|-------|---------|-------|-----------|------------|
| Agents | Count of `User` where `role: 'agent', companyId` | `plan.maxAgents` | e.g. "20 / 25 (80%)" | Green <70%, Yellow 70-90%, Red >90% |
| Sessions This Month | Count of `ChatSession` created this calendar month | `plan.maxSessionsPerMonth` | e.g. "2,400 / 5,000 (48%)" | Same color rules |
| Knowledge Items | Count of `KnowledgeItem` where `companyId` | `plan.maxKnowledgeItems` | e.g. "45 / 200 (23%)" | Same color rules |

### Section D: Staff Summary Table

| Column | Shows |
|--------|-------|
| Role | "Manager" / "Team Leader" / "Agent" |
| Count | Number of users with this role in company |
| Active | Number with `isActive: true` |
| Example names | First 3 names: "Omar, Sara, Ahmed..." |

### Section E: Recent Activity (Last 10 events)

| Column | Shows | Example |
|--------|-------|---------|
| Time | Relative time | "2 minutes ago" |
| Event | `eventLog.eventType` as readable text | "Ticket Created" |
| Details | `eventLog.metadata` summary | "#NQ-0217-0003 — Billing — Urgent" |

---

## 2.4 Plans Management View

**URL:** `/super-admin/plans`
**Purpose:** View and manage SaaS pricing plans

### Page Header

| Element | Content |
|---------|---------|
| Title | "Plans" |
| Subtitle | "Manage subscription plans and pricing" |
| Action | "+ Create Plan" button |

### Plans Display — Card Grid (4 cards in a row)

Each plan displays as a pricing card:

| Card Element | What It Shows | Example (Professional) |
|-------------|--------------|----------------------|
| Tier badge | `plan.tier` in uppercase | "PROFESSIONAL" — purple badge |
| Plan name | `plan.name` | "Professional" |
| Price | `plan.price` formatted + billing cycle | "2,999 EGP/mo" — large bold number |
| Annual note | If annual: show monthly equivalent | "or 29,990 EGP/year (save 17%)" |
| --- | Divider line | --- |
| Max Agents | `plan.maxAgents` | "25 agents" with people icon |
| Max Sessions | `plan.maxSessionsPerMonth` | "5,000 sessions/mo" with chat icon |
| Max KB Items | `plan.maxKnowledgeItems` | "200 knowledge items" with book icon |
| --- | Divider line | --- |
| Channels heading | "Channels:" | Bold label |
| Channel: Web | Is "web" in `plan.channels`? | "Web Chat" with green checkmark |
| Channel: Telegram | Is "telegram" in `plan.channels`? | "Telegram" with green checkmark |
| Channel: WhatsApp | Is "whatsapp" in `plan.channels`? | "WhatsApp" with green checkmark |
| --- | Divider line | --- |
| Features heading | "Features:" | Bold label |
| Feature: AI Chat | Is "ai_chat" in `plan.features`? | "AI Chat" — green check or red X |
| Feature: Sentiment | Is "sentiment" in `plan.features`? | "Sentiment Analysis" — green or red |
| Feature: QA | Is "qa_review" in `plan.features`? | "QA Review" — green or red |
| Feature: Analytics+ | Is "analytics_plus" in `plan.features`? | "Advanced Analytics" — green or red |
| --- | Divider line | --- |
| Active status | `plan.isActive` | Green "Active" or Gray "Inactive" |
| Companies count | How many active subscriptions use this plan | "5 companies" — muted text |
| --- | Divider line | --- |
| Edit button | Opens edit modal | [Edit] secondary button |
| Deactivate button | Toggle active state | [Deactivate] or [Activate] |

### Create/Edit Plan Modal

| Section | Field | Input Type | Shows |
|---------|-------|-----------|-------|
| Basic | Name | Text input | Plan display name |
| Basic | Tier | Select dropdown | "free", "starter", "professional", "enterprise" |
| Basic | Price | Number input + currency label "EGP" | Monthly price |
| Basic | Billing Cycle | Radio buttons | "Monthly" / "Annual" |
| Limits | Max Agents | Number input with stepper (+/-) | Maximum agents allowed |
| Limits | Max Sessions/Month | Number input | Maximum chat sessions per month |
| Limits | Max Knowledge Items | Number input | Maximum KB articles |
| Channels | Available Channels | Checkbox group | Web, Telegram, WhatsApp — each with checkbox |
| Features | Features | Checkbox group | ai_chat, sentiment, qa_review, analytics_plus, canned_responses, custom_branding |
| Settings | AI Enabled | Toggle switch | Whether AI is included in this plan |
| Settings | Active | Toggle switch | Whether plan is available for new subscriptions |

---

## 2.5 Subscriptions List View

**URL:** `/super-admin/subscriptions`

### Page Header

| Element | Content |
|---------|---------|
| Title | "Subscriptions" |
| Subtitle | "All company subscriptions and their status" |

### Filter Bar

| Filter | Options |
|--------|---------|
| Status | All, Trial, Active, Past Due, Expired, Cancelled |
| Plan | All, Free, Starter, Professional, Enterprise |
| Search | Company name |

### Table Columns

| Column | Shows | Format | Example |
|--------|-------|--------|---------|
| Company | `subscription.companyId → company.name` | Link to company detail | "Vodafone Egypt" |
| Plan | `subscription.planId → plan.name` | Text + tier color badge | "Professional" (purple) |
| Status | `subscription.status` | Colored badge | Green "active", Blue "trial", Orange "past_due", Red "expired", Gray "cancelled" |
| Start Date | `subscription.startDate` | "Jan 1, 2026" | Date format |
| End Date | `subscription.endDate` | "Jan 31, 2027" — Red text if ending in <7 days | Date format |
| Trial Ends | `subscription.trialEndsAt` | "Feb 28, 2026" or "—" | Only for trial |
| Auto Renew | `subscription.autoRenew` | Green "Yes" / Gray "No" | Toggle display |
| Actions | [Change Plan] [Change Status] | Dropdown buttons | |

### Action: Change Plan

- Opens modal with dropdown of all active plans
- Shows preview: "Change from Starter (999 EGP/mo) → Professional (2,999 EGP/mo)"
- Confirm button: "Update Plan"

### Action: Change Status

- Dropdown: activate, expire, cancel
- Each option requires confirmation dialog

---

## 2.6 Invoices List View

**URL:** `/super-admin/invoices`

### Page Header

| Element | Content |
|---------|---------|
| Title | "Invoices" |
| Subtitle | Total count: "124 total invoices" |
| Action | "+ Create Invoice" button |

### Summary Cards Row (4 small cards)

| Card | Shows | Example |
|------|-------|---------|
| Total Revenue | Sum of all paid invoices | "285,000 EGP" |
| Pending | Count + total of pending invoices | "5 pending (14,995 EGP)" |
| Overdue | Count + total of overdue invoices | "2 overdue (5,998 EGP)" — red accent |
| This Month | Sum of invoices paid this month | "45,000 EGP" |

### Filter Bar

| Filter | Options |
|--------|---------|
| Status | All, Draft, Pending, Paid, Overdue, Refunded |
| Type | All, Subscription, Setup, Addon, Usage |
| Company | Search/select |
| Date Range | Date picker |

### Table Columns

| Column | Shows | Format | Example |
|--------|-------|--------|---------|
| Invoice # | `invoice.invoiceNumber` | Monospace, link to detail | "INV-2026-0042" |
| Company | `invoice.companyId → company.name` | Link | "Vodafone Egypt" |
| Type | `invoice.type` | Colored badge: subscription=blue, setup=purple, addon=teal, usage=gray | "subscription" |
| Amount | `invoice.amount` + `invoice.currency` | Bold number, formatted | "2,999 EGP" |
| Status | `invoice.status` | Badge: draft=gray, pending=blue, paid=green, overdue=red, refunded=purple | Green "paid" |
| Due Date | `invoice.dueDate` | Date — RED if overdue (dueDate < today && status != paid) | "Feb 28, 2026" |
| Paid At | `invoice.paidAt` | Date or "—" | "Feb 15, 2026" |
| Actions | [View] [Mark Paid] | Buttons | |

### "Mark as Paid" Action

| Step | What Happens |
|------|-------------|
| 1 | Click "Mark Paid" button |
| 2 | Confirmation dialog: "Mark invoice INV-2026-0042 (2,999 EGP) as paid?" |
| 3 | On confirm: PATCH sets `status: 'paid'`, `paidAt: now()` |
| 4 | Badge animates from blue/red → green "Paid" |
| 5 | Toast: "Invoice marked as paid" |

---

# 3. Company Manager Views

## 3.1 Company Dashboard View

**URL:** `/manager/dashboard`
**Who Sees It:** `company_manager`
**Purpose:** Real-time overview of the company's contact center operations

### Page Header

| Element | Content |
|---------|---------|
| Title | "Dashboard" |
| Subtitle | Company name from logged-in user's company: "Vodafone Egypt" |
| Right side | Today's date and time, auto-updating |
| Date filter | Period selector: "Today" / "Last 7 days" / "Last 30 days" / "Custom" |

### Section A: KPI Cards Row (5 cards)

#### Card 1: Active Sessions

| Element | Shows | Example |
|---------|-------|---------|
| Icon | Chat bubble icon (green) | |
| Label | "Active Sessions" | |
| Value | Count of `ChatSession` where `companyId = mine, status = 'active'` | "23" |
| Trend | % change vs same period yesterday | "↑ 12% vs yesterday" — green |
| Live indicator | Green pulsing dot | Indicates real-time value |

#### Card 2: Open Tickets

| Element | Shows | Example |
|---------|-------|---------|
| Icon | Ticket icon (blue) | |
| Label | "Open Tickets" | |
| Value | Count of `Ticket` where `companyId = mine, status = 'open'` | "15" |
| Trend | vs yesterday | "↓ 3 fewer than yesterday" — green (fewer is good) |
| Sub-value | Urgent count | "3 urgent" — red small text if any urgent tickets exist |

#### Card 3: Online Agents

| Element | Shows | Example |
|---------|-------|---------|
| Icon | Headset icon | |
| Label | "Online Agents" | |
| Value | Count of agents currently connected via Socket.IO / Total agents | "5 / 8" |
| Visual | Mini avatar row showing who's online | 5 green-bordered avatars |
| Sub-value | Away count | "2 away" — yellow text |

#### Card 4: CSAT Today

| Element | Shows | Example |
|---------|-------|---------|
| Icon | Star icon | |
| Label | "CSAT Today" | |
| Value | Average `Rating.score` for today's ratings in this company | "4.2" — with star icon |
| Stars visual | Filled/empty stars showing the average | 4 filled + 1 partial star |
| Trend | vs yesterday | "↑ 0.3" — green |
| Sub-value | Total ratings today | "from 18 ratings" |

#### Card 5: Avg Response Time

| Element | Shows | Example |
|---------|-------|---------|
| Icon | Clock icon | |
| Label | "Avg Response Time" | |
| Value | Average of (`Ticket.firstResponseAt` - `Ticket.createdAt`) for today | "32 sec" |
| Trend | vs yesterday | "↓ 5 sec faster" — green |
| Target | Company target (if set) | "Target: 60 sec ✅" — green if met, red if not |

### Section B: Charts Row (2 charts)

#### Chart 1: Tickets Over Time (Left, 50%)

| Element | Shows |
|---------|-------|
| Type | Area/Line chart with 2 lines |
| X-axis | Last 7 days (or selected period), one point per day |
| Y-axis | Ticket count |
| Line 1 | "Created" — blue line: count of tickets created per day |
| Line 2 | "Resolved" — green line: count of tickets resolved per day |
| Fill | Light shaded area under each line |
| Hover | Tooltip: "Feb 17: 24 created, 19 resolved" |
| Legend | Below chart: blue dot "Created", green dot "Resolved" |

#### Chart 2: Tickets by Category (Right, 50%)

| Element | Shows |
|---------|-------|
| Type | Horizontal bar chart |
| Y-axis | Category names: Billing, Network, Complaint, Packages, Payment, Refund, Other |
| X-axis | Count |
| Bars | Colored bars with count label at end: "Billing ████████ 120" |
| Hover | Tooltip: "Billing: 120 tickets (27%)" |
| Sort | Descending by count |

### Section C: Charts Row (2 charts)

#### Chart 3: AI Resolution Rate (Left, 50%)

| Element | Shows |
|---------|-------|
| Type | Ring/Gauge chart |
| Value | Percentage of sessions resolved by AI without agent (sessions where `isAgentHandling` was never `true` and `status = 'closed'`) | "42%" |
| Ring | Filled portion = 42%, remaining = 58% |
| Center text | Percentage in large bold |
| Below | "Target: 40%" — green checkmark if above target, red X if below |
| Sub-text | "246 of 585 sessions resolved by AI" |

#### Chart 4: Channel Distribution (Right, 50%)

| Element | Shows |
|---------|-------|
| Type | Donut chart |
| Segments | One per channel from `ChatSession.channel` |
| Data | Count and % per channel |
| Labels | "Telegram: 380 (65%)", "Web Chat: 205 (35%)" |
| Colors | Telegram = blue, Web = green, WhatsApp = green, Messenger = purple |

### Section D: Recent Tickets Table (Full width)

| Element | Content |
|---------|---------|
| Section title | "Recent Tickets" |
| Right link | "View All →" — navigates to `/manager/tickets` |
| Rows shown | Latest 5 tickets sorted by `createdAt` descending |

| Column | Shows | Example |
|--------|-------|---------|
| Ticket # | `ticket.ticketNumber` | "#NQ-20260217-0003" — link to detail |
| Category | `ticket.category` | "Billing" badge |
| Priority | Priority icon + text | Red dot "Urgent" |
| Agent | `ticket.assignedTo → user.name` or "Unassigned" | "Omar Agent" or gray "Unassigned" |
| Time | Relative time since creation | "2 min ago" |
| Status | Status badge | Yellow "In Progress" |

---

## 3.2 Tickets List View

**URL:** `/manager/tickets`
**Purpose:** Full ticket management with filtering and actions

### Page Header

| Element | Content |
|---------|---------|
| Title | "Tickets" |
| Total count | "450 tickets" |
| Action | "+ Create Ticket" button (manual ticket creation) |

### Tab Bar (Status Filter)

| Tab | Label | Shows Count | Highlighted When |
|-----|-------|-------------|-----------------|
| 1 | "All" | Total tickets | Default tab |
| 2 | "Open" | `status: 'open'` count | Blue indicator |
| 3 | "In Progress" | `status: 'in_progress'` count | Yellow indicator |
| 4 | "Resolved" | `status: 'resolved'` count | Green indicator |
| 5 | "Closed" | `status: 'closed'` count | Gray indicator |

### Filter Bar

| Filter | Input Type | Options | Default |
|--------|-----------|---------|---------|
| Search | Text with search icon | Searches `ticketNumber`, customer name | Empty |
| Category | Multi-select dropdown | Billing, Network, Packages, Complaint, Payment, Refund, Other | All |
| Priority | Multi-select dropdown | Urgent, High, Medium, Low | All |
| Agent | Dropdown with search | All agents in company + "Unassigned" option | All |
| Channel | Dropdown | All, Telegram, Web | All |
| Date Range | Date range picker | Start date — End date | Last 30 days |
| Tags | Multi-select chips | All tags defined in company | None |

### Table — Every Column

| Column | Shows | Format | Sortable |
|--------|-------|--------|----------|
| Row # | Sequential number | "1", "2", "3"... | No |
| Ticket # | `ticket.ticketNumber` | "NQ-20260217-0003" — monospace, link | Yes |
| Customer | `ticket.userId → user.name` | "M. Mostafa" + small avatar | Yes |
| Channel | `ticket.channel` | Icon only: Telegram plane icon / Web globe icon | Yes |
| Category | `ticket.category` | Text badge: "Billing" | Yes |
| Priority | `ticket.priority` | Colored dot + text: "🔴 Urgent", "🟠 High", "🟡 Medium", "🔵 Low" | Yes |
| Agent | `ticket.assignedTo → user.name` | Agent name or italic gray "Unassigned" | Yes |
| Status | `ticket.status` | Colored badge (see status colors) | Yes |
| Tags | `ticket → TicketTag → Tag.name` | Colored mini badges: "[VIP] [Recurring]" | No |
| Created | `ticket.createdAt` | Relative: "2 min", "1 hr", "3 days" | Yes |
| First Response | Time until first agent response | "45 sec" or "—" (no response yet) | Yes |

### Row Hover Actions

| Action | Icon | What Happens |
|--------|------|-------------|
| View | Eye icon | Navigate to `/manager/tickets/:ticketId` |
| Quick Assign | Person+ icon | Dropdown of available agents, select to assign |
| Change Priority | Arrow icon | Dropdown: Urgent, High, Medium, Low |
| Change Status | Circle-check icon | Dropdown: Open, In Progress, Resolved, Closed |

### Bulk Actions (When checkboxes are selected)

| Action | What It Does |
|--------|-------------|
| Assign to... | Assign all selected tickets to one agent |
| Change status to... | Bulk status update |
| Add tag... | Add a tag to all selected tickets |
| Export | Download selected tickets as CSV |

---

## 3.3 Ticket Detail View

**URL:** `/manager/tickets/:ticketId`
**Purpose:** Full ticket context — conversation, metadata, actions, AI summary

### Page Header

| Element | Content |
|---------|---------|
| Back link | "← Back to Tickets" |
| Ticket number | "#NQ-20260217-0003" — large |
| Status badge | Current status as colored badge |
| Quick actions | [Resolve ✅] [Close 🔒] [Delete 🗑️] — right-aligned |

### Layout: Two Columns (60% left, 40% right)

#### LEFT COLUMN (60%)

##### Conversation Thread

Every message from `ChatSession.messages` (linked via `ticket.context.sessionId`) displayed as chat bubbles:

| Message Role | Bubble Alignment | Background Color | Avatar | Name Label |
|-------------|-----------------|-----------------|--------|-----------|
| `user` (customer) | Left | Light gray (#F3F4F6) | Customer initial or photo | Customer name + timestamp |
| `assistant` (AI) | Left | Light blue (#EFF6FF) | Robot icon | "AI Assistant" + timestamp |
| `agent` | Right | Light green (#F0FDF4) | Agent photo or initial | Agent name + timestamp |
| `system` | Center, no bubble | Muted text, small font | No avatar | Just italic text + timestamp |

Each message bubble shows:

| Element | Shows |
|---------|-------|
| Avatar | 32px circle: photo or first letter on colored background |
| Name | Role label or actual name |
| Content | Message text (supports markdown: bold, links, code blocks) |
| Timestamp | Time: "2:30 PM" — muted, below message |
| Delivery status (agent only) | Checkmark: ✓ sent, ✓✓ delivered |

Date separators between messages:

| Separator | Format | Example |
|-----------|--------|---------|
| Today's messages | "Today" | Centered gray text with horizontal lines |
| Yesterday | "Yesterday" | Same style |
| Older | Full date | "February 15, 2026" |

##### Agent Notes Section (Below conversation)

| Element | Shows |
|---------|-------|
| Section title | "Internal Notes" with lock icon (visible only to staff) |
| Existing notes | List of notes from `ticket.agentNotes[]` array |
| Each note | Agent avatar + name, note content, timestamp |
| Add note input | Text area: "Add an internal note..." with [Add Note] button |

#### RIGHT COLUMN (40%)

##### Panel 1: Ticket Information (Editable)

| Field | Current Value | Edit Control | On Change |
|-------|--------------|-------------|-----------|
| Status | `ticket.status` e.g. "In Progress" | Dropdown: Open / In Progress / Resolved / Closed | PATCH `/api/v1/admin/tickets/:id` → badge updates instantly |
| Priority | `ticket.priority` e.g. "Urgent" | Dropdown with colored options | PATCH → dot color changes |
| Category | `ticket.category` e.g. "Billing" | Dropdown | PATCH → badge updates |
| Channel | `ticket.channel` e.g. "Telegram" | Read-only display with icon | Not editable |
| Created | `ticket.createdAt` | Read-only | "Feb 17, 2026 at 2:30 PM" |
| First Response | `ticket.firstResponseAt` | Read-only | "Feb 17, 2026 at 2:32 PM" or "—" |
| Resolved At | `ticket.resolvedAt` | Read-only | Date or "Not resolved" |
| Resolution Time | `resolvedAt - createdAt` | Read-only, calculated | "1 hour 42 min" or "—" |
| Tags | List of tags from `TicketTag` | [+] button to add, X to remove | POST/DELETE tag API |

##### Panel 2: Customer Information

| Field | Shows | Example |
|-------|-------|---------|
| Avatar | Customer profile image or initial | 48px circle |
| Name | `user.name` | "Mohamed Mostafa" |
| Email | `user.email` | "tg_112074@natiq.com" |
| Phone | `user.phone` | "+20 100 123 4567" or "—" |
| Channel | How they contacted | "Telegram" with icon |
| Total Tickets | Count of all their tickets | "3 tickets" |
| History link | "View Customer History →" | Links to filtered tickets list |

##### Panel 3: Assigned Agent

| Field | Shows | Example |
|-------|-------|---------|
| Avatar | Agent photo or initial | 48px circle |
| Name | `ticket.assignedTo → user.name` | "Omar Agent" |
| Email | Agent email | "omar@natiq.com" |
| Department | Agent's department | "Technical Support" |
| Reassign | [Reassign ▼] dropdown | Dropdown of all agents in company |
| No agent | If `assignedTo` is null | "Unassigned — [Assign Agent ▼]" |

##### Panel 4: AI Summary

| Field | Shows | Example |
|-------|-------|---------|
| Detected Intent | `ticket.context.aiSummary.detectedIntent` or `session.summary.detectedIntent` | "billing_inquiry" |
| Confidence | `session.summary.confidence` | "85%" with color: green >80, yellow 50-80, red <50 |
| Summary Text | `ticket.context.aiSummary` or `session.summary.overview` | "Customer disputes a charge of 150 EGP on their February statement for a service they didn't subscribe to." |
| Empty state | If no AI summary available | "No AI summary available for this ticket" — muted text |

##### Panel 5: Attachments

| Element | Shows |
|---------|-------|
| List | Each attachment: file icon + name + size |
| Example | "📎 screenshot.png (2.3 MB)" — click to preview/download |
| Upload button | "[+ Upload File]" — opens file picker |
| Accepted types | Images (jpg, png, gif), Documents (pdf, docx), Max 10MB |
| Empty state | "No attachments" |

---

## 3.4 Chat Sessions List View

**URL:** `/manager/chat-sessions`
**Purpose:** View all customer conversations

### Page Header

| Element | Content |
|---------|---------|
| Title | "Chat Sessions" |
| Subtitle | "All customer conversations across channels" |

### Filter Bar

| Filter | Options |
|--------|---------|
| Status | All, Active, Closed |
| Channel | All, Telegram, Web |
| Handler | All, AI Only, Agent Handled |
| Search | Customer name, session ID |
| Date Range | Date picker |

### Table Columns

| Column | Shows | Format | Example |
|--------|-------|--------|---------|
| Session ID | `chatSession.sessionId` | Monospace, link | "CHAT-1803-9682" |
| Customer | `chatSession.userId → user.name` | Name + avatar | "Mohamed Mostafa" |
| Channel | `chatSession.channel` | Icon + text | Telegram icon "Telegram" |
| Status | `chatSession.status` | Dot: green "Active" / gray "Closed" | Green dot "Active" |
| Messages | `chatSession.messageCount` | Number | "24" |
| Handler | `chatSession.isAgentHandling` ? agent name : "AI" | Text | "AI" or "Omar Agent" |
| Linked Ticket | Ticket number if exists | Link or "—" | "#NQ-0217-0003" |
| Started | `chatSession.startedAt` | Relative time | "2 hours ago" |
| Last Activity | `chatSession.lastActivity` | Relative time | "5 min ago" |
| Duration | `endedAt - startedAt` or ongoing | Duration | "45 min" or "Ongoing" |

### Row Click Action

Navigate to conversation detail view showing full chat transcript (same layout as Ticket Detail conversation panel but without ticket-specific fields).

---

## 3.5 Staff Management View

**URL:** `/manager/agents`
**Purpose:** CRUD company staff (agents, team leaders)

### Page Header

| Element | Content |
|---------|---------|
| Title | "Staff Management" |
| Total | "18 staff members" |
| Action | "+ Add Staff Member" button |

### Tab Bar

| Tab | Shows | Count |
|-----|-------|-------|
| All | All staff | 18 |
| Agents | `role: 'agent'` | 12 |
| Team Leaders | `role: 'team_leader'` | 3 |
| Managers | `role: 'company_manager'` | 1 |

### Filter Bar

| Filter | Options |
|--------|---------|
| Search | Name, email |
| Department | All + list of departments |
| Status | All, Active, Inactive |

### Staff List — Each Row Shows

| Element | Shows | Example |
|---------|-------|---------|
| Avatar | Profile image or initials circle | 40px circle with "OA" |
| Name | `user.name` | "Omar Agent" — bold |
| Email | `user.email` below name | "omar@natiq.com" — muted |
| Role | `user.role` | Badge: "Agent" (blue), "Team Leader" (purple), "Manager" (gold) |
| Department | `user.departmentId → department.name` | "Technical Support" |
| Status | `user.isActive` | Green dot "Active" or Gray dot "Inactive" |
| Last Login | `user.lastLogin` | "2 hours ago" or "Never" |
| Quick Stats | Today's resolved tickets | "3 resolved today" — small muted text |
| Actions | Edit, Deactivate/Activate, View Performance | Icon buttons |

### Add/Edit Staff Modal

| Section | Field | Input | Required | Notes |
|---------|-------|-------|----------|-------|
| Personal | Name | Text | Yes | |
| Personal | Email | Email | Yes | Unique per company. Show error if duplicate |
| Personal | Phone | Phone input with country code | No | |
| Auth | Password | Password + strength meter | Yes (create) / No (edit) | Min 6 chars. Show: Weak/Medium/Strong |
| Auth | Confirm Password | Password | Yes if password filled | Must match |
| Work | Role | Select | Yes | company_manager, team_leader, agent |
| Work | Department | Select | No | List of company departments + "None" |
| Profile | Photo | File upload (image) | No | Preview circle, max 5MB |
| Settings | Active | Toggle | No | Default: On |

---

## 3.6 Knowledge Base View

**URL:** `/manager/knowledge-base`
**Purpose:** Manage the AI's knowledge — FAQs, packages, policies, complaint flows

### Page Header

| Element | Content |
|---------|---------|
| Title | "Knowledge Base" |
| Subtitle | "45 items powering your AI assistant" |
| Actions | "+ Add Item" button + "Re-embed All" button |

### Tab Bar

| Tab | Filter | Count | Icon |
|-----|--------|-------|------|
| All | No filter | 45 | List icon |
| Packages | `type: 'package'` | 12 | Box icon |
| FAQs | `type: 'faq'` | 20 | Question icon |
| Policies | `type: 'policy'` | 8 | Document icon |
| Complaint Flows | `type: 'complaint_flow'` | 5 | Flow icon |

### Search Bar

| Element | Behavior |
|---------|----------|
| Input | "Search knowledge base..." — searches `title`, `content`, `subtitle` |
| Debounce | 300ms delay before search |

### Item List — Each Row Shows

| Element | Shows | Example |
|---------|-------|---------|
| Type icon | Icon per type: 📦 package, ❓ faq, 📋 policy, 🔄 complaint_flow | 📦 |
| Title | `knowledgeItem.title` | "Premium 100GB Package" — bold |
| Subtitle | `knowledgeItem.subtitle` | "Best value for heavy data users" — muted |
| Type badge | `knowledgeItem.type` | Blue badge "Package" |
| Status | `knowledgeItem.isActive` | Green "Active" or Gray "Inactive" |
| Embedding status | Based on `lastEmbeddedAt` vs `updatedAt` | See below |
| Actions | [Edit] [Deactivate] [Delete] | Buttons |

### Embedding Status Visual

| Status | When | Visual |
|--------|------|--------|
| Embedded & current | `lastEmbeddedAt >= updatedAt` | Green checkmark + "Embedded Feb 15" |
| Needs re-embed | `lastEmbeddedAt < updatedAt` (content changed) | Yellow warning + "Content changed — needs re-embedding" |
| Never embedded | `lastEmbeddedAt` is null | Red X + "Not embedded yet" |

### Add/Edit Item Modal — Full Form

| Section | Field | Input | What Designer Should Show |
|---------|-------|-------|--------------------------|
| Basic | Type | Select: Package / FAQ / Policy / Complaint Flow | Changes which additional fields appear |
| Basic | Title | Text | "Premium 100GB Package" |
| Basic | Subtitle | Text (optional) | Short description |
| Basic | Slug | Auto-generated from title, editable | "premium-100gb-package" — monospace |
| Content | Content | Rich text editor (large textarea, 10+ lines) | The full knowledge content. Show character count |
| Content | Features | Tag input (type + enter to add) | Only for type=package: "100GB data", "Unlimited calls", etc. |
| Settings | Active | Toggle | Default: on |

---

## 3.7 AI Configuration View

**URL:** `/manager/settings/ai-config`
**Purpose:** Customize company AI behavior

### Page Header

| Element | Content |
|---------|---------|
| Title | "AI Configuration" |
| Subtitle | "Customize how your AI assistant responds to customers" |

### Form Layout

| # | Field | Input Type | Current Value Shown | What Designer Should Represent |
|---|-------|-----------|-------------------|-------------------------------|
| 1 | AI Enabled | Large prominent toggle | ON/OFF with color change | Green = on, Red = off. When OFF, show warning: "AI is disabled. All messages will be queued for agents." |
| 2 | Provider | Select dropdown | "Groq" / "OpenAI" / "Anthropic" | Logo of each provider next to name |
| 3 | Model | Select (changes based on provider) | Provider-specific model list | When Groq: "llama-3.3-70b-versatile", "llama-3.1-8b". When OpenAI: "gpt-4o", "gpt-4o-mini" |
| 4 | System Prompt | Large textarea (15+ lines visible) | The actual prompt text | Show character count: "1,247 / 4,000 chars". Show placeholder example prompt |
| 5 | Temperature | Horizontal slider | 0.0 to 1.0 with 0.1 steps | Left label "Precise (0.0)", right label "Creative (1.0)". Current value shown above thumb |
| 6 | Max Tokens | Number slider | 100 to 2000 | Show approximate word count: "~50 words" to "~500 words" next to slider |
| 7 | Languages | Multi-select chip input | Selected languages highlighted | "Arabic (ar)" "English (en)" "French (fr)" — click to toggle |
| 8 | Fallback Behavior | Radio button group | Selected option highlighted | 3 options with descriptions: "Escalate to agent — Create a ticket and notify available agents", "Retry — Try again with simpler prompt", "Default response — Send a predefined fallback message" |

### Test Panel (Right side or below)

| Element | Shows |
|---------|-------|
| Title | "Test Your AI" |
| Input | Text area: "Type a sample customer message..." |
| Button | [Test Response] |
| Output | AI response appears below with: response text, tokens used, response time |
| Note | "Uses current unsaved settings for testing" |

### Save Actions

| Button | Behavior |
|--------|----------|
| Save Changes | Primary button, saves all fields |
| Reset to Default | Secondary button, restores default values |
| Unsaved indicator | If any field changed, show yellow dot on Save button: "Unsaved changes" |

---

## 3.8 Analytics — Ticket Analytics View

**URL:** `/manager/analytics/tickets`

### Page Header

| Element | Content |
|---------|---------|
| Title | "Ticket Analytics" |
| Date range | Prominent date picker: Today / 7d / 30d / 90d / Custom |
| Export | [Export CSV] [Export PDF] buttons |

### KPI Cards Row (4 cards)

| Card | Label | Value Source | Example | Trend |
|------|-------|-------------|---------|-------|
| 1 | Total Tickets | Count in selected period | "450" | "↑ 12% vs previous period" |
| 2 | Avg Resolution Time | Average `(resolvedAt - createdAt)` for resolved tickets | "4.2 hours" | "↓ 18% faster" — green |
| 3 | Resolution Rate | Resolved / Total × 100 | "95%" | "↑ 3%" |
| 4 | Current Backlog | Tickets where status = open or in_progress | "23 tickets" | Red if >50, Yellow if >20, Green if <20 |

### Charts Grid

| Chart | Type | X-Axis | Y-Axis | Data |
|-------|------|--------|--------|------|
| Tickets by Status | Donut | — | — | Segments: open (blue), in_progress (yellow), resolved (green), closed (gray) |
| Tickets Over Time | Line/Area | Days in period | Count | 2 lines: Created (blue), Resolved (green) |
| Tickets by Category | Horizontal bar | Count | Category names | Sorted descending |
| Tickets by Channel | Donut | — | — | Segments per channel |
| Tickets by Priority | Stacked bar | Days | Count | 4 stacked segments: urgent (red), high (orange), medium (yellow), low (blue) |
| Top Agents by Resolution | Table | — | — | Columns: Rank, Agent Name, Resolved Count, Avg Resolution Time, CSAT |

---

## 3.9 Analytics — Sentiment View

**URL:** `/manager/analytics/sentiment`

### KPI Cards Row

| Card | Shows | Example |
|------|-------|---------|
| Avg Sentiment Score | Average `SentimentLog.score` (-1.0 to 1.0) | "0.42" — green positive |
| Positive % | Percentage of sessions with positive sentiment | "52%" |
| Neutral % | Percentage neutral | "31%" |
| Negative % | Percentage negative | "17%" — red if >25% |

### Charts

| Chart | Type | Shows |
|-------|------|-------|
| Sentiment Distribution | Donut | 3 segments: Positive (green), Neutral (yellow), Negative (red) |
| Sentiment Over Time | Stacked area | Daily counts of each sentiment type |
| Sentiment by Channel | Grouped bar | Telegram vs Web, showing positive/neutral/negative per channel |
| Most Negative Sessions | Table | Session ID (link), Customer name, Sentiment score, Linked ticket, "View" action |
| Agent Impact | Table | Agent name, Avg sentiment on their sessions, Comparison vs company average |

---

## 3.10 QA Reviews View

**URL:** `/manager/qa-reviews`

### Page Header

| Element | Content |
|---------|---------|
| Title | "QA Reviews" |
| Action | "+ New Review" button |

### Filter Bar

| Filter | Options |
|--------|---------|
| Status | All, Pending, Reviewed, Disputed |
| Agent | All agents |
| Reviewer | All team leaders / managers |
| Date | Date range |
| Score range | Slider: 1-10 |

### Table Columns

| Column | Shows | Format |
|--------|-------|--------|
| Session | `qaReview.sessionId` | Link to session |
| Agent | `qaReview.agentId → user.name` | Name + avatar |
| Reviewer | `qaReview.reviewerId → user.name` | Name + avatar |
| Overall Score | `qaReview.overallScore` | Large colored number: red <5, yellow 5-7, green >7. Show "/10" |
| Score Breakdown | Individual scores | Mini bars: Communication, Accuracy, Empathy, Resolution, Compliance |
| Status | `qaReview.status` | Badge: pending (blue), reviewed (green), disputed (red) |
| Date | `qaReview.createdAt` | "Feb 15, 2026" |
| Actions | [View Detail] | Button |

### Create QA Review View

| Section | What It Shows |
|---------|--------------|
| Session selector | Dropdown or search to find a session to review |
| Conversation preview | Full read-only chat transcript (scrollable, 40% height) |
| Agent info | Agent name, avatar, department |
| Scoring sliders | 5 sliders, each 1-10: Communication, Accuracy, Empathy, Resolution, Compliance |
| Auto-calculated | Overall Score: calculated average displayed in real-time |
| Strengths | Textarea: "What did the agent do well?" |
| Improvements | Textarea: "What could be improved?" |
| Notes | Textarea: "Additional notes (internal)" |
| Buttons | [Cancel] [Submit Review] |

---

## 3.11 Settings — Tags View

**URL:** `/manager/settings/tags`

### Tab Bar

| Tab | Shows |
|-----|-------|
| Ticket Tags | Tags where `entityType: 'ticket'` |
| Session Tags | Tags where `entityType: 'session'` |

### Tag List — Each Row

| Element | Shows | Example |
|---------|-------|---------|
| Color dot | `tag.color` as filled circle | 🔴 red circle |
| Name | `tag.name` | "VIP" |
| Entity Type | `tag.entityType` | "ticket" badge |
| Usage count | How many tickets/sessions use this tag | "Used on 12 tickets" |
| Actions | [Edit] [Delete] | Icon buttons |

### Create/Edit Tag Modal

| Field | Input | Notes |
|-------|-------|-------|
| Name | Text input, max 30 chars | |
| Color | Color palette (8-10 preset colors) + custom hex input | Show preview of badge with selected color |
| Entity Type | Radio: Ticket / Session | Not editable on edit |

---

## 3.12 Settings — Canned Responses View

**URL:** `/manager/settings/canned-responses`

### Table Columns

| Column | Shows | Example |
|--------|-------|---------|
| Title | `cannedResponse.title` | "Greeting - Arabic" |
| Shortcut | `cannedResponse.shortcut` | "/hello" — monospace font, blue text |
| Category | `cannedResponse.category` | Badge: "greeting" (green), "closing" (blue), "billing" (purple), "technical" (orange), "general" (gray) |
| Content Preview | First 80 chars of `cannedResponse.content` | "مرحبا بك! كيف يمكنني مساعدتك..." |
| Active | `cannedResponse.isActive` | Green dot or Gray dot |
| Created By | `cannedResponse.createdBy → user.name` | "Admin User" |
| Actions | [Edit] [Delete] [Deactivate] | Buttons |

### Create/Edit Modal

| Field | Input | Notes |
|-------|-------|-------|
| Title | Text | Descriptive name for the team |
| Shortcut | Text, auto-prefixed with "/" | Unique per company. Show error if duplicate |
| Category | Select dropdown | greeting, closing, billing, technical, general |
| Content | Large textarea | Show placeholder support note: "You can use {{customerName}} to insert the customer's name" |
| Active | Toggle | Default: on |
| Preview | Read-only box | Shows how the response will appear with placeholder values replaced |

---

## 3.13 Billing — Subscription View

**URL:** `/manager/billing/subscription`

### Current Plan Card

| Element | Shows | Example |
|---------|-------|---------|
| Plan name | `plan.name` in large text | "PROFESSIONAL" |
| Tier badge | `plan.tier` with tier color | Purple "professional" |
| Price | `plan.price` / `plan.billingCycle` | "2,999 EGP / month" |
| Status | `subscription.status` | Green "Active" badge |
| Renews on | `subscription.endDate` | "Renews: March 1, 2026" |
| Auto-renew | `subscription.autoRenew` | Toggle: ✅ On |

### Usage Section (3 Progress Bars)

| Meter | Label | Current / Limit | Bar | Example |
|-------|-------|----------------|-----|---------|
| Agents | "Agents" | `agentCount / plan.maxAgents` | Progress bar with percentage | "20 / 25 (80%)" — Yellow bar |
| Sessions | "Sessions this month" | `sessionCount / plan.maxSessionsPerMonth` | Progress bar | "2,400 / 5,000 (48%)" — Green bar |
| Knowledge | "Knowledge Items" | `kbCount / plan.maxKnowledgeItems` | Progress bar | "45 / 200 (23%)" — Green bar |

Bar colors: Green <70%, Yellow 70-90%, Red >90%. Show "Limit Reached!" alert badge at 100%.

### Features Included

| Feature | Shows |
|---------|-------|
| Each feature | Green checkmark ✅ if included, Red X ❌ if not |
| Example | "✅ AI Chat  ✅ Telegram  ✅ Web Chat  ✅ Sentiment  ✅ QA Review  ❌ WhatsApp  ❌ Custom Branding" |

### Upgrade CTA

| Element | Shows |
|---------|-------|
| If not on highest plan | "Upgrade to Enterprise for unlimited agents and all features" + [Contact Sales] button |
| If on Enterprise | "You're on our top plan" — muted text |

---

## 3.14 Billing — Invoices View

**URL:** `/manager/billing/invoices`

Same table structure as Super Admin Invoices (Section 2.6) but:
- Only shows invoices for the manager's company
- No "Mark as Paid" action (that's admin-only)
- Shows "Download PDF" action instead
- Shows payment instructions for pending invoices

---

# 4. Team Leader Views

## 4.1 Department Dashboard View

**URL:** `/team-leader/dashboard`
**Who Sees It:** `team_leader`
**Purpose:** Department-focused dashboard — same layout as Manager Dashboard but scoped to the leader's department

### Key Difference from Manager Dashboard

| Element | Manager | Team Leader |
|---------|---------|-------------|
| Data scope | Entire company | Only their department |
| Title | "Dashboard — Vodafone Egypt" | "Dashboard — Technical Support" (department name) |
| Active Sessions | All company sessions | Sessions handled by department agents |
| Open Tickets | All company tickets | Tickets assigned to department agents + unassigned |
| Online Agents | All company agents | Department agents only |
| CSAT | Company average | Department average |
| Charts | Company-wide data | Department data only |

### Additional Section: Department Team Status

| Element | Shows |
|---------|-------|
| Title | "My Team" |
| Agent cards | One card per department agent showing: avatar, name, status (Online/Away/Offline), current ticket count, CSAT today |
| Offline agents | Grayed out cards at the end |

---

## 4.2 Department Agents View

**URL:** `/team-leader/agents`
**Purpose:** View and manage department team members

Same layout as Manager Staff Management (Section 3.5) but:
- Only shows agents in the team leader's department
- Cannot create agents with different department
- Shows additional quick stats per agent:

| Extra Column | Shows | Example |
|-------------|-------|---------|
| Active Tickets | Count of `Ticket` assigned to agent with status open/in_progress | "3 active" |
| Resolved Today | Resolved count today | "5 today" |
| CSAT (7 days) | 7-day average CSAT for this agent | "4.3 ⭐" |
| Online | Socket connection status | Green dot or Gray dot |

---

## 4.3 QA Reviews View

Same as Manager QA Reviews (Section 3.10) — Team Leader is the primary reviewer. Filter defaults to their department's agents.

---

# 5. Agent Workspace Views

## 5.1 Agent Dashboard View

**URL:** `/agent/dashboard`
**Who Sees It:** `agent`
**Purpose:** Agent's personal workspace — see their tickets, queue, and stats at a glance

### Page Header

| Element | Shows | Example |
|---------|-------|---------|
| Greeting | "Good morning, [name]" / "Good afternoon, [name]" (based on time of day) | "Good morning, Omar" |
| Status selector | Dropdown in top-right: Online / Away / Offline | Green dot "Online ▼" |
| Date | Today's date | "Monday, Feb 17, 2026" |

### Section A: KPI Cards (4 cards)

#### Card 1: My Active Tickets

| Element | Shows | Example |
|---------|-------|---------|
| Label | "My Tickets" | |
| Value | Count of `Ticket` where `assignedTo = me, status IN ('open', 'in_progress')` | "5" |
| Breakdown | By status, small text | "2 open, 3 in progress" |
| Color | Red if >10, Yellow if >5, Green if ≤5 | Green for "5" |

#### Card 2: Resolved Today

| Element | Shows | Example |
|---------|-------|---------|
| Label | "Resolved Today" | |
| Value | Count of `Ticket` where `assignedTo = me, resolvedAt = today` | "3" |
| Target | If company sets daily target | "Target: 5" with progress indicator |

#### Card 3: Avg Response Time

| Element | Shows | Example |
|---------|-------|---------|
| Label | "Avg Response Time" | |
| Value | Average time from ticket creation to agent's first note/reply today | "45 sec" |
| Trend | vs last 7-day average | "↓ 12 sec faster" — green |

#### Card 4: My CSAT

| Element | Shows | Example |
|---------|-------|---------|
| Label | "My CSAT" | |
| Value | Average `Rating.score` for the agent's tickets (last 30 days) | "4.5" with stars |
| Total ratings | Count of ratings | "from 28 ratings" |

### Section B: My Active Tickets List

| Element | Content |
|---------|---------|
| Section title | "My Active Tickets" |
| Sort | By priority (urgent first), then by `createdAt` |

Each ticket row shows:

| Element | Shows | Example |
|---------|-------|---------|
| Priority dot | Colored dot | 🔴 |
| Ticket # | `ticket.ticketNumber` | "#NQ-0217-0003" |
| Category | `ticket.category` | "Billing" badge |
| Customer | `ticket.userId → user.name` | "M. Mostafa" |
| Time | Relative since creation | "2 min ago" |
| Last message preview | First 40 chars of last message | "I need to check my bi..." |
| Unread indicator | Blue dot if customer sent message agent hasn't viewed | 🔵 |
| Action | [Open →] button | Navigate to chat view |

### Section C: Unassigned Queue

| Element | Content |
|---------|---------|
| Section title | "Unassigned Queue" |
| Count | "12 tickets in queue" |
| Sort | By priority (urgent first), then by `createdAt` (oldest first) |

Each queue row shows:

| Element | Shows | Example |
|---------|-------|---------|
| Priority dot | Colored dot | 🔴 |
| Ticket # | `ticket.ticketNumber` | "#NQ-0217-0005" |
| Category | `ticket.category` | "Complaint" badge |
| Priority | `ticket.priority` text | "Urgent" |
| Channel | `ticket.channel` icon | Telegram icon |
| Time waiting | Since creation | "8 min" |
| Action | [Claim] button | Yellow/primary button |

#### Claim Button Behavior

| Step | What Happens in View |
|------|---------------------|
| 1 | Agent clicks [Claim] |
| 2 | Button shows spinner, text: "Claiming..." |
| 3 | API: POST `/api/v1/agent/tickets/:ticketId/claim` |
| 4a | Success: Ticket slides from queue → "My Active Tickets" with animation. Toast: "Ticket #NQ-0217-0005 claimed successfully" |
| 4b | Fail (already claimed): Toast error: "This ticket was already claimed by another agent." Ticket fades out of queue |
| 5 | Queue count decrements |

### Section D: Weekly Performance Mini-Chart

| Element | Shows |
|---------|-------|
| Type | Small horizontal bar chart |
| Data | Tickets resolved per day, Mon-Fri (or current work week) |
| Today highlighted | Today's bar in accent color |
| Example | "Mon: 2, Tue: 4, Wed: 3, Thu: 5" |

---

## 5.2 Agent Active Chat View (Core Screen)

**URL:** `/agent/tickets/:ticketId/chat`
**Who Sees It:** `agent`
**Purpose:** Real-time conversation with customer — the agent's primary workspace

### Layout: 3-Column

- Left sidebar (15%): Ticket list
- Center (60%): Chat area
- Right panel (25%): Customer & ticket info

### LEFT SIDEBAR: Ticket List

#### "My Tickets" Section

Each ticket card shows:

| Element | Shows | Position |
|---------|-------|----------|
| Active indicator | Filled circle (●) — means assigned to agent | Left edge |
| Ticket # | `ticket.ticketNumber` shortened | "NQ-0003" |
| Priority | Colored dot | Right of ticket # |
| Customer name | First name + last initial | Below ticket # |
| Last message | First 25 chars of last message | Below name, muted text |
| Time | Relative time of last message | Right side |
| Unread badge | Blue circle with count if unread messages | Right side, overlapping |
| Selected state | Highlighted background when this is the active chat | Light primary color background |

#### "Unassigned" Section (Below my tickets)

| Element | Shows |
|---------|-------|
| Separator | "── Unassigned ──" divider |
| Ticket cards | Same format but hollow circle (○) instead of filled |
| Claim action | Small [Claim] button on each card |
| New ticket indicator | Subtle pulse animation when new ticket appears |

### CENTER: Chat Area

#### Chat Header

| Element | Shows | Position |
|---------|-------|----------|
| Ticket # | `ticket.ticketNumber` | Left |
| Customer name | `ticket.userId → user.name` | Next to ticket # |
| Channel icon | Telegram/Web icon | Next to name |
| Priority badge | Colored priority badge | Right area |
| Status | Current ticket status badge | Right area |

#### Message List (Scrollable area)

Each message shows:

| Message Type | Position | Background | Avatar | Name | Time |
|-------------|----------|-----------|--------|------|------|
| Customer (`role: 'user'`) | Left | Light gray #F3F4F6 | Customer photo or initial circle | Customer name | Below message, left |
| AI (`role: 'assistant'`) | Left | Light blue #EFF6FF | Robot icon 🤖 | "AI Assistant" | Below message, left |
| This Agent (`role: 'agent'`, my messages) | Right | Light green #F0FDF4 | My avatar | "You" | Below message, right |
| Other Agent (`role: 'agent'`, different agent) | Right | Light teal | Their avatar | Their name | Below message, right |
| System (`role: 'system'`) | Center | No bubble, just text | No avatar | — | Centered timestamp |

Each message bubble contains:

| Element | Shows | Notes |
|---------|-------|-------|
| Avatar | 28px circle | Only on first message in consecutive group |
| Name | Sender name | Only on first message in consecutive group |
| Content | Message text | Supports line breaks. Long messages fully visible (no truncation) |
| Timestamp | "2:30 PM" | Small, muted, below content |
| Delivery indicator | Only for agent's own messages | ✓ Sent, ✓✓ Delivered (if channel supports) |

Day separators:

| When | Shows |
|------|-------|
| First message of a new day | Centered text: "Today", "Yesterday", or "February 15, 2026" with horizontal lines on both sides |

#### Scroll Behavior

| Behavior | Detail |
|----------|--------|
| On open | Auto-scroll to most recent message |
| New message received | If scrolled to bottom: auto-scroll. If scrolled up: show blue "New message ↓" floating button |
| Load older messages | Scroll to top → loading spinner → older messages appear above |

#### Message Input Bar (Bottom)

| Element | Shows | Behavior |
|---------|-------|----------|
| Attach button (📎) | Left side of input | Opens file picker. Accepted: images, documents. Max 10MB |
| Text input | "Type a message..." | Auto-expanding textarea (1 to 4 lines) |
| Send button (→) | Right side | Primary color. Disabled when input empty. Click or Enter to send |
| Shift+Enter | — | New line within message (doesn't send) |
| "/" trigger | When agent types "/" | Opens canned response autocomplete dropdown above input |

#### Canned Response Autocomplete

| Trigger | What Appears |
|---------|-------------|
| Agent types "/" | Dropdown list of all canned responses |
| Agent types "/bi" | Filtered list showing only shortcuts starting with "bi" |
| Each item shows | Shortcut ("/billing-check") + title + first 40 chars of content |
| Click or Enter | Response content inserted into message input. Agent can edit before sending |
| Escape | Close dropdown |

### RIGHT PANEL: Info & Actions

#### Customer Section

| Field | Shows | Example |
|-------|-------|---------|
| Avatar | Large (56px) customer photo or initial | Circle with "MM" |
| Name | `user.name` | "Mohamed Mostafa" |
| Email | `user.email` | "tg_112074@natiq.com" |
| Phone | `user.phone` | "+20 100 123 4567" or "No phone" |
| Channel | How they're chatting | "Telegram" with icon |
| Total tickets | All tickets from this customer | "3 tickets" — link to filtered list |

#### Ticket Info Section

| Field | Shows | Editable |
|-------|-------|----------|
| Status | Current status badge | No (use quick actions) |
| Priority | Colored dot + text | No |
| Category | Category badge | No |
| Created | Date + time | No |
| First Response | Time of first agent reply or "—" | No |
| Duration | Time since ticket created | Auto-updating: "23 min" |

#### Quick Actions Section

| Button | Label | Behavior |
|--------|-------|----------|
| Green button | "Resolve ✅" | Confirmation: "Mark this ticket as resolved?" → POST `/api/v1/agent/tickets/:id/resolve` |
| Gray button | "Close 🔒" | Confirmation: "Close this ticket?" → POST `/api/v1/agent/tickets/:id/close` |
| Blue button | "Transfer ↗" | Opens dropdown of other agents. Select → ticket reassigned |

#### Canned Responses Section

| Element | Shows |
|---------|-------|
| Title | "Quick Replies" |
| List | All canned responses: shortcut + title |
| Click | Inserts content into message input |
| Search | Mini search box to filter responses |

#### Knowledge Base Section

| Element | Shows |
|---------|-------|
| Title | "Knowledge Base" |
| Search | "Search articles..." input |
| Results | Matching KB items: title + first 50 chars of content |
| Click | Expands to show full article content in a panel |
| Insert | [Insert in chat] button on each result — adds content to message input |

---

## 5.3 Agent Performance View

**URL:** `/agent/performance`
**Purpose:** Agent's personal performance metrics and feedback

### Page Header

| Element | Content |
|---------|---------|
| Title | "My Performance" |
| Period | Select: "This Week" / "This Month" / "Last 30 Days" / "Custom" |

### KPI Cards (5 cards)

| Card | Label | Value | Trend | Example |
|------|-------|-------|-------|---------|
| 1 | Tickets Resolved | Count resolved in period | vs previous period | "42 ↑12%" |
| 2 | Avg Handle Time | Average resolution time | vs previous | "28 min ↓15%" |
| 3 | CSAT Score | Average rating score | vs previous | "4.5 ⭐ ↑0.3" |
| 4 | First Call Resolution | % resolved on first interaction | vs previous | "88% ↑5%" |
| 5 | QA Score | Latest QA review overall score | vs previous review | "8.2/10 ↑0.5" |

### Charts Section

| Chart | Type | Shows |
|-------|------|-------|
| Resolved Over Time | Bar chart | Daily count of resolved tickets |
| Response Time Trend | Line chart | Daily average first response time |
| Category Breakdown | Donut | % of tickets by category the agent handles |

### Recent Ratings Section

| Element | Shows |
|---------|-------|
| Title | "Recent Customer Ratings" |
| Each row | Star rating (1-5 visual), customer comment (if any), customer name, ticket #, date |
| Example | "⭐⭐⭐⭐⭐ 'Great service, very helpful!' — Mohamed Mostafa — #NQ-0217-0003 — Feb 17" |
| Empty state | "No ratings yet. Ratings appear after customers rate their experience." |

### Latest QA Review Section

| Element | Shows |
|---------|-------|
| Title | "Latest QA Review" |
| Reviewer | Name + avatar of who reviewed |
| Date | When reviewed |
| Scores | 5 mini progress bars: Communication (8/10), Accuracy (9/10), Empathy (7/10), Resolution (9/10), Compliance (10/10) |
| Overall | "Overall: 8.6 / 10" — large, colored |
| Strengths | Text from reviewer |
| Improvements | Text from reviewer |
| Link | "View All Reviews →" |
| Empty state | "No QA reviews yet. Your team leader will review your conversations." |

---

## 5.4 Agent Profile View

**URL:** `/agent/profile`
**Purpose:** Agent can view and update their personal info

### Profile Display & Edit Form

| Section | Field | Shows | Editable | Input Type |
|---------|-------|-------|----------|-----------|
| Photo | Profile Image | Current photo in large circle (96px). Camera icon overlay on hover | Yes | Click to upload (JPG/PNG/WebP, max 5MB). Show preview before save |
| Personal | Name | `user.name` | Yes | Text input |
| Personal | Email | `user.email` | No (read-only, grayed out) | Display only |
| Personal | Phone | `user.phone` | Yes | Phone input with +country code |
| Work | Department | `user.departmentId → department.name` | No (read-only) | Display only |
| Work | Role | `user.role` | No (read-only) | Display: "Agent" badge |
| Work | Company | `user.companyId → company.name` | No (read-only) | Display only |
| Password | Current Password | Hidden | Yes (required to change password) | Password input |
| Password | New Password | Hidden | Yes | Password input + strength meter: Weak (red) / Medium (yellow) / Strong (green) |
| Password | Confirm Password | Hidden | Yes | Password input, must match new password |

### Password Change Rules

| Rule | Display |
|------|---------|
| Must enter current password | Error if wrong: "Current password is incorrect" |
| New password min 6 chars | Inline validation as user types |
| Passwords must match | Error: "Passwords do not match" |
| Success | Toast: "Password updated successfully" |

### Buttons

| Button | Label | Behavior |
|--------|-------|----------|
| Save | "Save Changes" | Validates → PATCH `/api/v1/agent/profile` → Toast success |
| Cancel | "Cancel" | Reverts all changes to current values |

---

# 6. Customer Web Chat Widget Views

## 6.1 Chat Bubble (Collapsed State)

**Position:** Fixed, bottom-right corner of the host website, 20px margin
**Size:** 56px circle

| Element | Shows |
|---------|-------|
| Icon | Chat bubble icon 💬 or company logo |
| Color | Company primary color (from settings) |
| Animation | Gentle pulse on first visit (draws attention) |
| Unread badge | Red circle with count if there are unread messages | "3" |
| Hover | Slightly enlarge (scale 1.05) + tooltip: "Chat with us" |
| Click | Opens chat window with slide-up animation |

---

## 6.2 Chat Window (Expanded State)

**Size:** 370px wide × 520px tall (desktop). Full-screen on mobile.
**Position:** Fixed bottom-right, 20px margin (desktop)

### Header Bar

| Element | Shows | Example |
|---------|-------|---------|
| Company name | From company settings | "Vodafone Egypt" |
| Company logo | Small logo (24px) | Logo image |
| Status indicator | Green dot "Online" or Red dot "Offline" | Based on working hours or agent availability |
| Minimize button | "—" icon | Collapses back to bubble |
| Close button | "✕" icon | Closes chat, confirmation if active session |

### Welcome Screen (No Active Session)

| Element | Shows |
|---------|-------|
| Welcome message | "Welcome! How can we help you today?" — large text |
| Company description | "We're here to help with billing, technical support, and more." |
| Start button | [Start Chat] — primary button |
| Common topics | Quick buttons: "Billing Question", "Technical Issue", "Complaint", "Other" — clicking one starts chat with that as first message |
| Working hours note | If outside hours: "We're currently offline. Leave a message and we'll respond during working hours (9:00 AM - 5:00 PM)" |

### Active Chat View

| Element | Position | Shows |
|---------|----------|-------|
| Messages area | Main scrollable area | All messages in chronological order |
| AI messages | Left aligned | Company color background, robot icon, "AI Assistant" label |
| Agent messages | Left aligned | Slightly different shade, agent name + avatar, "Agent Omar" label |
| Customer messages | Right aligned | Gray background, no avatar needed |
| System messages | Center | Small muted text: "Agent Omar has joined the conversation" |
| Typing indicator | Left side, below last message | Three dots animation "..." when AI or agent is typing |
| Time stamps | Below each message | "2:30 PM" |
| Day separators | Between days | "Today", "Yesterday" |

### Message Input Area

| Element | Shows |
|---------|-------|
| Attachment button | 📎 icon — opens file picker |
| Text input | "Type a message..." — auto-expanding (1 to 3 lines max) |
| Send button | Arrow → icon. Disabled when empty. Enabled when text present |
| Enter key | Sends message |
| Shift+Enter | New line |

### Chat States

| State | What Customer Sees |
|-------|-------------------|
| **AI handling** | Messages from "AI Assistant" with robot icon. Typing indicator shows "AI is typing..." |
| **Waiting for agent** | System message: "Connecting you with an agent..." with loading spinner. Then "Agent Omar has joined the conversation." |
| **Agent handling** | Messages from agent with their name. Agent messages have slightly different color than AI |
| **Agent typing** | "Agent Omar is typing..." with dots animation |
| **Outside working hours** | Header shows "Offline". Welcome screen shows hours. Customer can still send messages (queued) |
| **Session ended** | "This conversation has ended." + [Start New Chat] button + Rating prompt |

---

## 6.3 Rating Prompt View

Appears after session ends or ticket is resolved.

| Element | Shows |
|---------|-------|
| Title | "How was your experience?" |
| Stars | 5 clickable stars — empty by default, fill on hover/click |
| Selected feedback | Dynamic text based on stars: 1="Poor", 2="Fair", 3="Good", 4="Very Good", 5="Excellent" |
| Comment | Optional textarea: "Any additional feedback? (optional)" |
| Submit | [Submit Rating] button — disabled until stars selected |
| Skip | [Skip] text link — closes without rating |
| After submit | "Thank you for your feedback!" with checkmark animation. Auto-closes after 2 seconds |

---

# 7. Shared Views & Overlays

## 7.1 Notification Center Dropdown

**Trigger:** Click bell icon in top bar (all panels)

### Dropdown Panel

| Element | Shows |
|---------|-------|
| Header | "Notifications" + [Mark All Read] link |
| Unread count | Badge on bell icon: number of `Notification` where `isRead: false` |
| Each notification | Icon (by type) + title + message + relative time |
| Unread visual | Bold text + blue dot on left edge |
| Read visual | Normal weight text, no dot |
| Click | Navigate to related entity (ticket, review, etc.) and mark as read |
| Load more | "View All Notifications →" link at bottom → full notifications page |
| Empty state | "All caught up! No new notifications." with checkmark |
| Real-time | New notifications slide in from top with subtle animation + notification sound |

### Notification Types — Visual per Type

| Type | Icon | Title Example | Click Navigates To |
|------|------|-------------|-------------------|
| ticket_assigned | Blue ticket icon | "New ticket assigned" | Ticket detail |
| ticket_updated | Yellow pencil icon | "Ticket #NQ-0003 updated" | Ticket detail |
| ticket_resolved | Green check icon | "Ticket #NQ-0003 resolved" | Ticket detail |
| new_message | Green chat icon | "New message from customer" | Chat view |
| qa_review | Purple clipboard icon | "QA review available" | QA review detail |
| performance | Orange chart icon | "Weekly performance summary" | Performance view |
| system | Gray info icon | "System maintenance scheduled" | — |

---

## 7.2 Notification Full Page View

**URL:** `/[role]/notifications`

### Table/List

| Column | Shows |
|--------|-------|
| Status | Blue dot for unread, empty for read |
| Icon | Type-specific icon |
| Title | `notification.title` |
| Message | `notification.message` |
| Time | Relative time + full date on hover |
| Action | Click row to navigate |

### Filters

| Filter | Options |
|--------|---------|
| Status | All, Unread, Read |
| Type | All, Ticket, Message, QA, Performance, System |

---

## 7.3 Global Search Overlay

**Trigger:** Click search icon in top bar or keyboard shortcut Ctrl+K

### Search Modal

| Element | Shows |
|---------|-------|
| Input | Large search input: "Search tickets, sessions, agents, knowledge base..." |
| Scope tabs | [All] [Tickets] [Sessions] [Agents] [Knowledge Base] |
| Results | Grouped by type with 3 results each |
| Ticket result | Ticket # + customer name + status badge + time |
| Session result | Session ID + customer name + channel + status |
| Agent result | Name + avatar + role + department |
| KB result | Title + type + first 60 chars |
| Click | Navigate to the entity's detail page |
| Keyboard | Arrow keys to navigate, Enter to select, Esc to close |
| Empty | "No results for '[query]'" |

---

## 7.4 Confirmation Dialog (Reusable)

| Element | Shows |
|---------|-------|
| Overlay | Dark semi-transparent backdrop |
| Dialog box | White card, centered, max 400px wide |
| Icon | Warning triangle (yellow) for caution, red circle for destructive |
| Title | Action-specific: "Resolve Ticket?" / "Deactivate Agent?" / "Delete Tag?" |
| Message | Description of consequences: "This will mark ticket #NQ-0217-0003 as resolved and notify the customer." |
| Cancel button | "Cancel" — secondary/outline style |
| Confirm button | Action-specific label + color: "Resolve" (green), "Deactivate" (orange), "Delete" (red) |

---

## 7.5 Empty States (Per View)

| View | Illustration | Heading | Message | Action |
|------|-------------|---------|---------|--------|
| Tickets List | Support desk illustration | "No tickets yet" | "Tickets will appear here when customers need help through any channel." | — |
| Chat Sessions | Chat bubbles illustration | "No conversations" | "Conversations will appear when customers start chatting through your widget or Telegram." | [Copy Widget Code] |
| Agent Queue | High-five illustration | "Queue is clear!" | "No unassigned tickets. Great work, team!" | — |
| Knowledge Base | Books/brain illustration | "Knowledge base is empty" | "Add FAQs, packages, and policies to train your AI assistant." | [+ Add First Item] |
| Notifications | Checkmark illustration | "All caught up!" | "No new notifications. We'll notify you when something happens." | — |
| QA Reviews | Clipboard illustration | "No reviews yet" | "QA reviews will appear here once team leaders start reviewing sessions." | [+ Start a Review] |
| Agent Performance | Chart illustration | "No data yet" | "Performance data will build up as you handle tickets." | — |
| Invoices | Receipt illustration | "No invoices" | "Invoices will be generated based on your subscription." | — |
| Search Results | Magnifying glass illustration | "No results" | "Try different keywords or check your filters." | [Clear Filters] |
| Staff List (filtered) | People illustration | "No staff match" | "No staff members match your current filters." | [Clear Filters] |

---

## 7.6 Loading States

| View Type | Loading Visual |
|-----------|---------------|
| Full page load | Centered spinner with "Loading..." text |
| Table loading | Skeleton rows: 5 rows of animated gray bars matching column widths |
| Card loading | Skeleton card: gray rectangle with shimmer animation |
| Chart loading | Gray rectangle with shimmer + "Loading chart..." text |
| KPI card loading | Gray bar for value, smaller bar for label |
| Chat messages loading | 3 skeleton message bubbles (alternating left/right) |
| Button loading | Spinner replaces button text, button disabled |
| Inline action | Small spinner next to the element being updated |

---

## 7.7 Error States

| Error Type | What the User Sees |
|-----------|-------------------|
| Page load failure | Full-page error: red icon + "Something went wrong" + "We couldn't load this page. Please try again." + [Retry] button |
| API error on action | Toast notification: red, "Failed to [action]. Please try again." + dismiss button |
| Network offline | Top banner (full width, yellow): "You're offline. Changes will sync when you reconnect." |
| Session expired | Modal overlay: "Your session has expired. Please log in again." + [Login] button |
| Permission denied | Full page: lock icon + "Access Denied" + "You don't have permission to view this page." + [Go to Dashboard] button |
| 404 Not Found | Full page: "Page not found" + "The page you're looking for doesn't exist or has been moved." + [Go to Dashboard] button |

---

*End of Screen Views Specification*
