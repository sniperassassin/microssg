# Gym Micro-SaaS — Product & Business Plan

> **North Star:** Never lose a gym renewal because someone forgot to follow up.

**This document covers *what* to build and *why*** — scope, decisions, and rationale. It changes rarely.

For ***how* and *when*** — numbered tasks, status, exit criteria, open decisions, risk register — see **[EXECUTION.md](EXECUTION.md)**.

---

## Table of Contents

**Part I — Business Context**
1. [Product Vision](#1-product-vision)
2. [Target Customer](#2-target-customer)
3. [Problems to Solve](#3-problems-to-solve)
4. [Core Product Positioning](#4-core-product-positioning)

**Part II — Product Scope**

5. [MVP Scope (P0)](#5-mvp-scope-p0)
6. [Phase 1 Scope (P1)](#6-phase-1-scope-p1) — includes [Freeze, deferred](#65-membership-freeze--deferred-pending-customer-research)
7. [Phase 2 and Later](#7-phase-2-and-later)
8. [Development Priorities](#8-development-priorities)
9. [Things NOT to Build Initially](#9-things-not-to-build-initially)
10. [Product Principle](#10-product-principle)

**Part III — Technical Design**

11. [Technology Strategy](#11-technology-strategy)
12. [Architecture](#12-architecture)
13. [Multi-Tenancy](#13-multi-tenancy)
14. [Scheduling & the Expiry Model](#14-scheduling--the-expiry-model)
15. [Core Automation Engine](#15-core-automation-engine)
16. [WhatsApp Provider Abstraction](#16-whatsapp-provider-abstraction)
17. [Non-Functional Requirements](#17-non-functional-requirements)
18. [Testing Strategy](#18-testing-strategy)

**Part IV — Go To Market**

19. [Sales Demo Strategy](#19-sales-demo-strategy)
20. ["Simulate Expiry" Demo Feature](#20-simulate-expiry-demo-feature)
21. [Demo Data](#21-demo-data)
22. [Sales Strategy](#22-sales-strategy)
23. [Competitive Differentiation](#23-competitive-differentiation)
24. [Pricing Experiment](#24-pricing-experiment)
25. [Onboarding](#25-onboarding)
26. [MVP User Journey](#26-mvp-user-journey)

**Part V — Validation & Measurement**

27. [Business Validation Strategy](#27-business-validation-strategy)
28. [Pilot Strategy](#28-pilot-strategy)
29. [Product Analytics](#29-product-analytics)
30. [Revenue Attribution](#30-revenue-attribution)
31. [Key Success Metric](#31-key-success-metric)
32. [First Version Success Criteria](#32-first-version-success-criteria)

**Part VI — Execution**

33. [Development Roadmap](#33-development-roadmap) — task detail in [EXECUTION.md](EXECUTION.md)
34. [Immediate Next Steps](#34-immediate-next-steps)
35. [Decision Log](#35-decision-log)

---

# Part I — Business Context

## 1. Product Vision

Build a lightweight gym-management SaaS focused on one core business outcome:

> **Help gym owners increase membership renewals, reduce missed payments, and bring inactive members back using automated WhatsApp follow-ups.**

The first version will be a responsive web application that works well on Android and iPhone browsers. Native Android/iOS apps can be considered later.

The product should not initially try to become a complete gym ERP. It should focus on the highest-value workflows around:

**Membership → Attendance → Expiry → WhatsApp → Renewal**

---

## 2. Target Customer

Primary customer:

- Independent gyms
- Small and medium-sized fitness centers
- Gym owners who currently use Excel, notebooks, WhatsApp, or basic gym software
- Gyms with approximately 100–2,000 members
- Gyms where the owner/receptionist personally handles renewals and follow-ups

Initial market assumption:

- India
- English-first product
- WhatsApp-first communication
- INR pricing
- Responsive web app

Potential future markets can be evaluated after product-market fit.

---

## 3. Problems to Solve

### 3.1 Membership Expiry

Gym owners often manually check which memberships are expiring.

**Solution.** Automatically identify upcoming expiries and send WhatsApp reminders.

Example sequence:

- 30 days before expiry
- 15 days before expiry
- 7 days before expiry
- 3 days before expiry
- 1 day before expiry
- After expiry

### 3.2 Missed Payments

Members forget or delay payments.

**Solution.** Track amount due, due date, paid amount, outstanding amount, and overdue status. Send automated WhatsApp reminders.

### 3.3 Member Drop-Off

Members stop visiting but the gym may not notice until much later.

**Solution.** Track attendance and identify members who have not visited for 7, 14, or 30 days. Send re-engagement messages and alert gym staff.

### 3.4 Lost Renewals

A member's membership expires and nobody follows up.

**Solution.** Automated renewal campaigns before and after expiry.

### 3.5 Forgotten Leads

Gym enquiries from walk-ins, WhatsApp, phone calls, Instagram, etc. may not be followed up.

**Solution.** Basic CRM covering lead creation, lead source, interested plan, lead status, follow-up date, notes, and conversion to member.

Statuses:

`New → Contacted → Trial → Joined → Lost`

### 3.6 Trial Conversion

Free trials and short trials are easy to forget.

**Solution.** Track trial dates and automatically follow up before the trial ends.

### 3.7 Poor Revenue Visibility

Gym owners may not know how much revenue is due, how many memberships are expiring, how much revenue is at risk, how many renewals happened, or how many members are inactive.

**Solution.** Simple business-focused dashboard.

---

## 4. Core Product Positioning

Do NOT primarily position the product as:

> "Gym Management Software"

Instead position it as:

> **"We help gyms increase membership renewals and reduce member drop-offs using automated WhatsApp follow-ups."**

Alternative positioning:

> **"Never lose a gym renewal because someone forgot to follow up."**

The product should demonstrate business outcomes rather than simply listing software features.

---

# Part II — Product Scope

## 5. MVP Scope (P0)

The MVP should contain only the features necessary to validate the business. Everything in this section is P0.

### 5.1 Authentication & Roles

- Gym owner login
- Staff login
- Password reset
- Basic role-based access

Potential roles:

- Owner
- Manager
- Receptionist
- Trainer

For the first MVP, Owner + Staff may be sufficient.

### 5.2 Dashboard

The dashboard should answer:

> "Where am I losing money today?"

Example:

```text
XYZ FITNESS

Members              487
Active               392
Expiring this week    18
Expired               31
Payment Due           14
Inactive              27

THIS MONTH

New Members           32
Renewals              41
Revenue          ₹2,84,000

ACTION REQUIRED

18 memberships expire this week
14 payments are overdue
27 members haven't visited in 14 days
8 leads haven't been contacted
```

#### Revenue at Risk

The most important dashboard metric.

```text
18 memberships expire in next 7 days

Average membership value: ₹4,000

Revenue at Risk: ₹72,000
```

CTA:

> Recover ₹72,000

This metric should connect the product directly to business value. See [§30 Revenue Attribution](#30-revenue-attribution) for how this number is framed.

### 5.3 Member Management

Each member should have a profile.

```text
Rahul Sharma

Membership
-------------------------
Plan:        3 Months
Start:       01 Jul 2026
Expiry:      30 Sep 2026
Status:      Active

Payment
-------------------------
Plan Price:  ₹6,000
Paid:        ₹6,000
Due:         ₹0

Attendance
-------------------------
Visits:      18
Last Visit:  22 Sep

PT
-------------------------
Trainer:     Amit
Sessions:    8 / 12
```

Actions: WhatsApp · Renew · Payment · Attendance · Edit

Member fields:

- Name
- Phone number
- Email (optional)
- Date of birth (optional)
- Gender (optional)
- Address (optional)
- Emergency contact (optional)
- Membership plan
- Start date
- Expiry date
- Payment information
- Attendance
- Notes
- Trainer/PT information

Avoid collecting unnecessary personal information in MVP.

#### Membership Status

A membership is always in exactly one state:

```text
Active  →  Expired
```

| Status | Meaning | Reminders | Attendance |
|---|---|---|---|
| Active | Normal | Yes | Allowed |
| Expired | Past effective expiry date | Post-expiry only | Blocked |

Status is **derived** from the effective expiry date (§5.5), not stored as an editable field. This keeps it impossible for status and date to disagree, and means a new state — such as Frozen (§6.5) — can be added later without backfilling rows.

### 5.4 Membership Plans

Gym owners can create plans.

```text
Monthly       ₹2,000
3 Months      ₹5,000
6 Months      ₹8,000
12 Months     ₹12,000
```

Support:

- Plan name
- Duration
- Price
- Joining fee
- Discount
- Tax/GST if applicable
- Extension
- Complimentary days

Freeze policy is **deferred** — see §6.5.

### 5.5 Expiry Computation

Extension days and complimentary days both move a membership's expiry date outward. They share one mechanism.

#### Do not overwrite `expiry_date`

Store each adjustment as its own record and compute expiry:

```text
effective_expiry
  = base_expiry
  + Σ (complimentary days)
  + Σ (extension days)
```

A `MembershipAdjustment` row holds the type, day count, reason, and who applied it.

This gives three things a mutated date cannot:

1. **Disputes are resolvable.** Staff can show a member exactly why their expiry is 14 Oct.
2. **Adjustments are reversible.** Applied in error, an adjustment is removed and expiry recomputes — no arithmetic to undo by hand.
3. **One rule, not several.** Every future adjustment type — including freeze (§6.5) — is a new row type, not a new code path or a schema migration.

Everything downstream reads `effective_expiry`, never `base_expiry`: the dashboard, reminders, the daily scan, and derived status (§5.3).

This composes with the daily scan (§14): the scan reads computed expiry, so an adjustment made yesterday is automatically respected today. Nothing needs cancelling or rescheduling.

### 5.6 WhatsApp Automation

This is the primary differentiator.

Automated membership messages:

- Welcome
- Membership activated
- 30 days before expiry
- 15 days before expiry
- 7 days before expiry
- 3 days before expiry
- 1 day before expiry
- Membership expired
- Renewal confirmation

Example:

```text
Hi Rahul 👋

Your gym membership expires in 7 days.

Don't let your fitness journey stop 💪

Renew your membership today.

[Renew Now]
```

> **MVP note.** WhatsApp is implemented against a mock provider for the MVP — see [§16](#16-whatsapp-provider-abstraction). No Meta account is required to build, test, or demo the scheduling logic.

### 5.7 Payment Notifications

Automations:

- Payment due
- Payment overdue
- Payment received
- Renewal payment confirmation
- Receipt/invoice

Example:

```text
Hi Rahul,

Your ₹2,000 membership payment is pending.

Please complete your payment to continue your membership.

[Pay Now]
```

Payment gateway integration (Razorpay) is **P2** — the MVP tracks payments as records, it does not collect money. Do not build a custom payment processor.

### 5.8 Attendance

MVP:

- Manual check-in
- Member search
- Mark attendance
- View attendance history
- Last visit date

Check-in must be blocked for Expired memberships.

Future: QR attendance, RFID, biometric integration, face recognition. Do not build biometric functionality in the initial MVP.

### 5.9 Inactivity Detection

Automatically identify:

```text
No visit for 7 days
No visit for 14 days
No visit for 30 days
```

Possible automated message:

```text
Hey Rahul 👋

We haven't seen you at XYZ Fitness recently.

Everything okay? 💪

Your fitness journey is waiting for you!
```

Staff dashboard alert:

```text
Rahul Sharma
Inactive for 14 days
[WhatsApp] [View Member]
```

### 5.10 Reports

MVP reports:

- Active members
- New members
- Expired members
- Renewals
- Revenue
- Outstanding payments
- Attendance
- Inactive members
- Leads
- Lead conversion

Export: CSV and Excel. PDF can be added later.

### 5.11 Data Import

Many gyms already have Excel data.

> Import Members from CSV/Excel

Expected columns:

```text
Name
Phone
Plan
Start Date
Expiry Date
Amount
Amount Paid
Amount Due
```

Provide preview before import, validation errors, duplicate detection, and an import summary:

```text
500 records uploaded

Valid: 472
Duplicates: 18
Invalid phone: 7
Missing expiry date: 3
```

This is important for onboarding existing gyms.

---

## 6. Phase 1 Scope (P1)

### 6.1 Lead Management

Basic CRM.

Lead fields: Name · Phone · Source · Interested plan · Notes · Assigned staff · Follow-up date · Status

Statuses:

```text
New
Contacted
Trial
Joined
Lost
```

Funnel:

```text
100 Leads
   ↓
70 Contacted
   ↓
42 Trial
   ↓
28 Joined
```

Track conversion rate.

### 6.2 Lead Follow-Up

```text
Lead: Rahul Sharma
Status: Contacted
Last Contact: 2 days ago
Next Follow-Up: Today
```

Notification:

> Follow up with Rahul Sharma today.

Future automation: new lead acknowledgement, trial reminder, trial ending reminder, post-trial follow-up, lost lead reactivation.

### 6.3 Trial Management

Support trial start date, trial end date, trial type, interested plan, and conversion status.

```text
Trial starts Monday

Monday:
Welcome to XYZ Fitness 💪

Friday:
Your trial ends tomorrow.
Would you like to continue with a membership?
```

### 6.4 Revenue Recovery Dashboard

Track:

- Expiring memberships
- Renewal reminders sent
- Renewals completed
- Renewal rate
- Revenue recovered
- Revenue at risk
- Inactive members reactivated

```text
This Month

Memberships Expiring       50
Renewed                    32

Renewal Rate               64%

Revenue at Risk        ₹2,00,000
Revenue Recovered      ₹1,28,000
```

See [§30 Revenue Attribution](#30-revenue-attribution) for the decision on how this figure is presented.

### 6.5 Membership Freeze — deferred pending customer research

**Status: not in MVP.** Deferred until gym owners have been interviewed about how they actually handle this today.

#### What it is

A member pays for three months, then travels for two weeks, or is injured, or has surgery. Without a freeze, that time burns — they paid for days they could not use. Freeze pauses the clock instead, and expiry moves out by the frozen duration.

```text
Rahul: 01 Jul → 30 Sep (3 months)
Freezes 10 Sep → 24 Sep (14 days, travelling)
New expiry: 14 Oct
```

#### Why gyms want it

- Costs the gym **nothing in cash** — the money is already collected, so it is pure goodwill
- The alternative is worse: a refund request, or a member who quietly does not renew
- "Can I freeze if I travel?" is a routine objection at the sales desk; being able to say yes closes it
- A member who lost three weeks of a twelve-week plan is measurably less likely to renew — freeze protects exactly the renewal this product exists to capture

#### Why it is deferred

The mechanism is simple; the **policy** is not, and policy varies by gym. Building a freeze feature without knowing how real gyms grant, limit, and price it risks shipping rules nobody uses and then having to change them under live data.

#### What to ask gym owners

1. Do you currently allow members to pause a membership?
2. How does a member request it — in person, WhatsApp, phone?
3. Who approves it, and is it ever refused?
4. How many days do you typically allow, and how often?
5. Do you charge for it?
6. Do you require proof (travel, medical)?
7. What stops members from abusing it?
8. Roughly how many members ask per month?

#### Likely shape, when built

| Rule | Plausible default | Purpose |
|---|---|---|
| Max freeze days per membership | 15–30 | Caps the giveaway |
| Minimum freeze duration | 7 days | Prevents weekend-by-weekend gaming |
| Freezes allowed per membership | 1–2 | Same |
| Freeze fee | Usually free | Optional revenue |

These are estimates, not decisions. Confirm them against interview answers.

#### What MVP must not preclude

Three things keep freeze cheap to add later, and all are already in the MVP design:

1. **Additive expiry computation** (§5.5) — freeze becomes a new `MembershipAdjustment` type, not a schema change
2. **Derived membership status** (§5.3) — adding a `Frozen` state requires no row backfill
3. **Daily scan rather than pre-queued jobs** (§14) — a freeze applied today is respected tomorrow with no jobs to cancel

When freeze is built, it will also need to suppress **inactivity alerts** (§5.9) — otherwise the system sends "we haven't seen you in 14 days 👋" to a member whose absence the gym formally approved.

---

## 7. Phase 2 and Later

### 7.1 PT / Trainer Management

```text
Amit

Members: 32
PT Members: 12

Today's Sessions:
10:00 Rahul
11:00 Priya
17:00 Arjun
```

PT package:

```text
Rahul Sharma

Package: 20 sessions
Used: 13
Remaining: 7

Next session:
Tomorrow 6 PM
```

Automated session reminder:

> Reminder: Your PT session with Amit is tomorrow at 6 PM 💪

### 7.2 Campaigns & Offers

Allow owners to select audiences: expired members, members expiring soon, inactive members, members in a particular age group, PT members, former members, leads, specific membership plans.

```text
Audience:
☑ Expired members
☑ Inactive members
☐ PT members
☐ Active members

Message:
_________________________

[Send WhatsApp Campaign]
```

Possible campaign:

> 💪 We miss you! Come back this week and get 20% OFF your membership renewal.

Important:

- Respect WhatsApp Business/API policies
- Use approved templates where required
- Track message delivery/status
- Do not implement spam-like behavior

### 7.3 WhatsApp Conversational Sales

```text
Member:
I want to renew

System:
Great! Here are our plans:

1 Month — ₹2,000
3 Months — ₹5,000
6 Months — ₹8,000

[Pay Now]
```

Potential future capabilities: renew membership, check expiry, check payment due, book PT session, ask gym timings, ask membership prices, request support.

This can eventually become an AI/automation layer.

---

## 8. Development Priorities

### 8.1 P0 — Must Have

- Multi-tenant authentication
- Gym setup
- Member CRUD
- Membership plans
- Membership expiry
- Extension / complimentary days
- Payment tracking
- Attendance
- Dashboard
- WhatsApp abstraction + mock provider
- WhatsApp templates
- Expiry automation
- Payment reminders
- Basic reports
- CSV/Excel import

### 8.2 P1 — Important

- Lead CRM
- Trial management
- Inactivity automation
- Renewal analytics
- Revenue at risk
- Revenue recovered
- Campaigns
- Membership freeze (§6.5 — after customer research)

### 8.3 P2 — Later

- Live WhatsApp Cloud API integration
- PT management
- Trainer management
- QR attendance
- Razorpay payment links
- Advanced campaigns
- Multiple branches
- Member portal
- Native Android/iOS apps

### 8.4 P3 — Future

- AI WhatsApp assistant
- Conversational sales
- AI support
- Workout plans
- Diet plans
- Biometric integrations
- RFID
- Face recognition

---

## 9. Things NOT to Build Initially

Avoid spending time on:

- Native mobile apps
- Complex accounting
- Payroll
- Inventory
- Full CRM
- Advanced workout planning
- Diet planning
- Biometric hardware
- Face recognition
- Social features
- Community feed
- Marketplace
- Complex AI

First prove:

> **Gym owners will pay for automated member follow-up and renewal management.**

---

## 10. Product Principle

Every feature should answer one of these questions. Does it help the gym:

1. Get more members?
2. Keep more members?
3. Get paid faster?
4. Reduce manual work?
5. Understand the business better?

If a feature does not meaningfully support one of these outcomes, it should probably not be part of the early MVP.

---

# Part III — Technical Design

## 11. Technology Strategy

### 11.1 Platform

Responsive web application.

Goals:

- Desktop support
- Android browser support
- iPhone browser support
- Mobile-friendly UI
- PWA-ready architecture

Do not build native Android/iOS apps initially. Native apps can be considered after product-market fit.

### 11.2 Technology Stack

| Layer | Technology | Why |
|---|---|---|
| Frontend | **Next.js (App Router) + TypeScript** | Responsive web/PWA foundation |
| Backend | **Next.js route handlers + service layer** | Same codebase; Auth.js works natively |
| UI | **Tailwind CSS + shadcn/ui** | Fast SaaS UI development |
| Database | **PostgreSQL** | Strong relational fit for this domain |
| ORM | **Prisma** | Fast development, migrations, tenant guard |
| Auth | **Auth.js** | Session handling inside Next.js |
| Scheduling | **Vercel Cron → daily scan route** | One job per day; no worker needed |
| WhatsApp | **Provider interface + mock (MVP)** → Meta Cloud API | Ship without a Meta account |
| Payments | **Record-keeping only (MVP)** → Razorpay | Gateway is P2 |
| Storage | **S3-compatible** | Documents/invoices later |
| Monitoring | **Sentry** | Errors + production debugging |
| Analytics | **PostHog** | Product usage/activation analytics |
| Deployment | **Vercel + managed Postgres** | Single deploy target for MVP |
| CI/CD | **GitHub Actions** | Automated testing/deployment |
| Testing | **Vitest + Playwright** | Unit + E2E |

#### Deferred, with trigger conditions

| Technology | Add when |
|---|---|
| **Redis** | Dashboard queries become slow enough to need caching |
| **BullMQ** | A single daily scan run cannot finish inside the function timeout (~5 min), or per-message retry/backoff is needed |
| **Separate backend service** | The service layer outgrows Next.js, or a non-web client appears |

These were in the original stack proposal and remain the right answers — they are simply not needed on day one. A daily scan is one cron invocation; BullMQ exists to manage thousands of individually-scheduled future jobs, which is precisely the model rejected in §14.

### 11.3 Code Organisation

Business logic lives in a service layer, not in route handlers:

```text
src/
  app/                    Next.js routes and UI
    api/
      cron/daily-scan/    Invoked by Vercel Cron
  server/
    services/             Business logic (membership, expiry, notification)
    providers/            WhatsAppProvider interface + mock + live
    db/                   Prisma client + tenant extension
```

Route handlers parse input, call a service, and return. The daily scan calls the **same** service functions as the API, so expiry logic exists in exactly one place.

This costs nothing now and preserves the option to extract a standalone backend later without a rewrite.

### 11.4 Deployment Topology (MVP)

```text
Vercel
  ├── Next.js app (UI + API routes)
  └── Vercel Cron → /api/cron/daily-scan (daily, 09:00 IST)

Managed Postgres
```

One deploy target. Revisit when BullMQ is added, at which point a worker process needs its own host.

---

## 12. Architecture

```text
                    ┌─────────────────────┐
                    │ Responsive Web App  │
                    │ Desktop/Android/iOS │
                    └──────────┬──────────┘
                               │
                    ┌──────────▼──────────┐
                    │  Next.js API layer  │
                    └──────────┬──────────┘
                               │
              ┌────────────────┼────────────────┐
              │                │                │
       ┌──────▼──────┐  ┌──────▼──────┐  ┌──────▼──────┐
       │ PostgreSQL  │  │ Daily Scan  │  │  Services   │
       │  (Prisma)   │  │   (Cron)    │  │   Layer     │
       └─────────────┘  └──────┬──────┘  └─────────────┘
                               │
                      ┌────────▼─────────┐
                      │ WhatsAppProvider │
                      │  mock  →  live   │
                      └──────────────────┘
```

The architecture must support:

- Multi-tenancy
- Scheduled notifications
- Audit logs
- Role-based access
- Secure API
- Payment integration (later)
- WhatsApp provider abstraction

---

## 13. Multi-Tenancy

The system is a multi-tenant SaaS from day one. **The gym is the tenant.** Members are records belonging to a gym; they do not log in (no member portal until P2).

### 13.1 Strategy — shared schema

A single database and schema, with `gym_id` on every gym-owned table.

**Core entities:**

```text
Gym
User
Role
Member
MembershipPlan
Membership
MembershipAdjustment
Payment
Attendance
Lead
Trial
Trainer
PTPackage
PTSession
NotificationTemplate
Notification
Campaign
AuditLog
```

### 13.2 Enforcement — Prisma client extension

Shared schema has one serious failure mode: a single missing `WHERE gym_id = ?` exposes one gym's members to another. That is a one-line mistake, and the worst possible bug in this product.

Discipline alone will eventually fail. **Every query goes through a Prisma client extension that injects `gym_id` from the request context automatically**, so a leaking query cannot be written by accident.

| Approach | Catches | Cost |
|---|---|---|
| Developer discipline | Nothing reliably | Free |
| **Prisma extension** ← chosen | Everything through Prisma | ~1 day |
| Postgres RLS | Everything, including raw SQL | ~2 days |

Postgres RLS remains available as defence-in-depth if raw SQL is ever introduced.

**Rules:**

- Every gym-owned table has a non-null `gym_id`
- Composite indexes lead with `gym_id`
- Tenant isolation has dedicated tests (§18)

---

## 14. Scheduling & the Expiry Model

### 14.1 Daily scan, not pre-queued jobs

Reminders are found by a **daily scan**, not queued in advance when a membership is created.

The rejected alternative fails on mutation:

> Rahul expires 30 Sep. On 23 Sep the system queues the "7 days left" message. On 25 Sep the gym grants him two weeks' extension, moving expiry to 14 Oct. **The queued message still fires**, telling him he is expiring when he is not.

Pre-queuing means every date change must locate and cancel stale jobs. A daily scan asks "who expires in exactly 7 days *as of today*?" — so extensions, complimentary days, and any adjustment type added later (including freeze, §6.5) need no special handling at all.

### 14.2 The scan

```text
Every day at 09:00 IST:

  for each gym:
    for each active membership:
      days_left = effective_expiry - today
      if days_left in (30, 15, 7, 3, 1): send expiry reminder
      if days_left < 0 and not yet notified: send expired message

    for each member:
      if days_since_last_visit in (7, 14, 30): send inactivity message

    for each payment:
      if due today or overdue: send payment reminder
```

`effective_expiry` is the computed value from §5.5, never the stored base date.

### 14.3 Timezone

All scheduling is **IST**. This is not incidental:

- A scan at midnight UTC runs at 05:30 IST — members receive renewal reminders while asleep
- "Expiring today" counts flip over at the wrong moment for the owner looking at the dashboard

Store membership dates as **dates, not timestamps**. Run the scan on an IST schedule. Send during business hours (09:00 IST is the current default).

### 14.4 Idempotency

The scan must be safe to run twice — a retry, a manual trigger, or a duplicate cron fire must not double-send. Every send is recorded in the `Notification` table before dispatch, and the scan skips anything already recorded for that member, template, and date.

---

## 15. Core Automation Engine

Build the notification system so new workflows can be added without hardcoding every message.

```text
Trigger:
membership.expiring

Condition:
days_until_expiry = 7

Action:
send_whatsapp_template(expiry_7_day)
```

Triggers:

```text
membership.created
membership.expiring
membership.expired
payment.due
payment.overdue
payment.completed
attendance.inactive
lead.created
lead.followup_due
trial.expiring
pt.session.tomorrow
```

This automation engine is the reusable core of the product. Every P1 and P2 messaging feature — inactivity, trials, lead follow-up, PT reminders, campaigns — is the same engine with a different trigger. Built properly once, those become configuration rather than new code.

**Messages must be data, not branches.** Adding a reminder should be a row, not a deploy.

---

## 16. WhatsApp Provider Abstraction

Business logic must never couple to a specific WhatsApp provider.

```text
interface WhatsAppProvider
  sendTemplate(to, templateKey, variables) → { messageId, status }
  sendMessage(to, text)                    → { messageId, status }
  getMessageStatus(messageId)              → status
```

### 16.1 MVP — mock provider

No Meta Business account is required to build or demo this product.

`MockWhatsAppProvider` implements the interface above, logs the full payload, and returns a 200-equivalent success. It is selected by environment variable, so switching to the live provider later is configuration, not code.

This sequences well because the hard logic is provider-independent. What actually needs to be correct is *scheduling*: "given a membership expiring 30 Sep, the 7-day reminder fires on 23 Sep at 09:00 IST, and shifts correctly if the expiry date is adjusted." That is fully testable against the mock.

**Every attempt is persisted to the `Notification` table** — member, template, scheduled date, sent date, status, provider response — not only logged. Same effort, and it provides:

- The delivery-status UI, already built when the live provider arrives
- The dedupe guard that makes the scan idempotent (§14.4)
- An audit trail
- Assertable rows for E2E tests, instead of scraping stdout

### 16.2 Live integration (P2)

When the Meta account exists, implement `MetaCloudWhatsAppProvider` against the same interface and flip the environment variable.

**Prerequisites — start these early, they have lead times:**

- Verified Meta Business account (document checks, multi-day)
- WhatsApp Business phone number
- **Template approval.** Any message sent to a member who has not messaged the gym in the last 24 hours — which is every reminder in this product — must use a template submitted to Meta *in advance* and approved:

  ```text
  Write template → Submit to Meta → Review → Approved / Rejected
                                    minutes to ~48 hours
  ```

  Each message in §5.6 is a separate template. Rejections are common (promotional copy marked as utility, vague variable placeholders) and require edit-and-resubmit. Changing wording later means resubmitting and waiting again.

- Per-message fees apply; see §24 on accounting for them in pricing

**Do not rely on unofficial WhatsApp automation methods.**

Provider selection criteria when the time comes: official Business API support, pricing, template support, delivery status, webhooks, Indian business support, reliability, compliance.

---

## 17. Non-Functional Requirements

### 17.1 Security

- Secure authentication
- Password hashing
- HTTPS
- Authorization checks
- **Tenant isolation** (§13.2)
- Audit logs for important actions
- Secure secrets management
- No API keys in source code

### 17.2 Reliability

- The daily scan must be idempotent and safe to re-run (§14.4)
- WhatsApp failures must not break core workflows
- Failed messages must be logged and visible
- Payment webhooks must be idempotent (when payments land)
- Scheduled jobs must be observable — a scan that silently stops running is the worst failure mode in this product, because nothing visibly breaks

### 17.3 Scalability

Initial scale target:

- 100 gyms
- 100,000 total members

Architecture should allow growth beyond this without major redesign.

---

## 18. Testing Strategy

### 18.1 Unit (Vitest)

The expiry and scheduling rules, which are where the real complexity lives:

- Expiry computed from base + complimentary + extension days
- Removing an adjustment recomputes expiry correctly
- Status derives from effective expiry, not the stored base date
- Reminder fires at exactly 30/15/7/3/1 days
- Reminders follow effective expiry after an adjustment, not the original date
- IST boundaries — a membership expiring "today" behaves correctly at 00:00 and 23:59 IST

### 18.2 Integration

- Daily scan against a seeded database produces exactly the expected `Notification` rows
- Running the scan twice produces no duplicates
- **Tenant isolation: a query in Gym A's context can never return Gym B's rows** — this deserves its own dedicated test suite

### 18.3 E2E (Playwright)

- Owner signup → create gym → create plans → import members → dashboard
- Add member → verify welcome notification row
- Advance clock → verify correct reminder rows appear
- Extend a membership → verify reminders shift to the new date
- Demo scenarios A–E (§21) all reachable

Time-dependent tests use an injectable clock, not real waiting.

---

# Part IV — Go To Market

## 19. Sales Demo Strategy

The salesman should NOT start by explaining every feature. The demo should start with a business problem.

**Step 1.** Ask: *How many members do you have?*

**Step 2.** Ask: *How many memberships expire every month?*

**Step 3.** Ask: *How many of those members usually renew?*

**Step 4.** Show:

```text
50 memberships expiring this month

Average membership:
₹4,000

Potential revenue:
₹2,00,000
```

Then demonstrate:

```text
Member approaching expiry
        ↓
Automated WhatsApp reminder
        ↓
Member receives message
        ↓
Member clicks Renew
        ↓
Payment
        ↓
Membership renewed
```

The product should make this demo possible in under 5 minutes.

---

## 20. "Simulate Expiry" Demo Feature

For a test member:

```text
Membership expires tomorrow
```

Button:

> Simulate 1 Day Before Expiry

The system triggers the configured WhatsApp workflow immediately rather than waiting for the daily scan.

This makes the product far easier for the salesman to demonstrate. It is also useful in development against the mock provider.

---

## 21. Demo Data

Create a demo gym with realistic data.

```text
Gym:                XYZ Fitness
Members:            487
Active:             392
Expiring this week: 18
Expired:            31
Payment due:        14
Inactive:           27
Leads:              43
Trials:             12
```

Demo scenarios:

| Scenario | Setup |
|---|---|
| A | Member expiring tomorrow |
| B | Payment overdue |
| C | Member inactive for 14 days |
| D | New lead requiring follow-up |
| E | Trial ending tomorrow |

The salesman should be able to demonstrate each scenario quickly.

---

## 22. Sales Strategy

The first salesman should sell the outcome.

Opening:

> "How do you currently track members whose memberships are about to expire?"

Then:

> "How many of those members do you have every month?"

Then:

> "How many usually renew?"

Then demonstrate the automation.

Core pitch:

> "Instead of your receptionist manually checking every member, our system automatically identifies expiring memberships and follows up through WhatsApp. Your team only needs to handle members who actually respond."

---

## 23. Competitive Differentiation

1. WhatsApp-first
2. Renewal-focused
3. Revenue-at-risk dashboard
4. Automated inactivity recovery
5. Very simple onboarding
6. Designed for small/medium gyms
7. Easy salesman demo
8. Responsive web app
9. ROI-focused reporting
10. Automation instead of data-entry-heavy gym management

Avoid competing solely on number of features, number of screens, or complex enterprise functionality.

---

## 24. Pricing Experiment

### Starter — ₹999/month

- Up to 300 members
- Membership management
- Attendance
- Basic WhatsApp reminders

### Growth — ₹1,999/month

- Up to 1,000 members
- WhatsApp automation
- Leads
- Payment tracking
- Campaigns
- Reports

### Pro — ₹3,999/month

- Large member base
- Multiple trainers
- Advanced campaigns
- Payment integration
- Advanced analytics
- Multiple branches

These prices are hypotheses and should be validated through actual sales conversations.

WhatsApp API message costs should be accounted for separately or included with reasonable usage limits.

---

## 25. Onboarding

The salesman should be able to onboard a gym quickly.

> Target: gym operational within 15–30 minutes.

```text
✓ Gym created
✓ Plans created
✓ WhatsApp connected
✓ Members imported
✓ Templates configured
✓ First automation enabled
```

---

## 26. MVP User Journey

### Gym Owner

```text
Sign Up
   ↓
Create Gym
   ↓
Create Membership Plans
   ↓
Connect WhatsApp
   ↓
Import/Add Members
   ↓
Configure Message Templates
   ↓
Dashboard
```

### Member

```text
Gym joins member
   ↓
Membership created
   ↓
Welcome WhatsApp
   ↓
Attendance tracking
   ↓
Expiry reminders
   ↓
Renewal
   ↓
Payment
   ↓
Membership extended
```

---

# Part V — Validation & Measurement

## 27. Business Validation Strategy

### Stage 1 — Customer Interviews

Talk to at least 15–20 gym owners.

1. How do you currently manage members?
2. How do you know who is expiring?
3. How do you remind members?
4. How many members expire each month?
5. What percentage renew?
6. How do you track attendance?
7. How do you follow up with inactive members?
8. How do you handle leads?
9. What software do you currently use?
10. What do you pay for it?
11. What do you dislike about it?
12. Would automated WhatsApp renewal follow-up be valuable?
13. How much would you pay monthly?
14. Would you allow a salesperson to demo this?
15. What would make you switch from your current process?

Do not lead the interview toward confirming the product idea.

---

## 28. Pilot Strategy

> Target: 5–10 gyms

Offer an early-adopter/pilot plan.

Measure:

- Number of members
- Number of expiring memberships
- Messages sent
- Messages delivered
- Renewals
- Renewal rate
- Revenue recovered
- Owner/staff usage
- Manual work reduced

Interview each pilot customer every 1–2 weeks.

---

## 29. Product Analytics

Product usage:

- Number of gyms
- Active gyms
- Members per gym
- Messages sent
- Message delivery rate
- Expiry reminders sent
- Renewals
- Revenue recovered
- Leads created
- Lead conversion
- Daily active gym users
- Monthly active gym users
- Churn
- Subscription revenue

SaaS metrics: MRR · ARR · CAC · LTV · Churn · Activation rate · Retention · Expansion revenue

---

## 30. Revenue Attribution

### Decision

**The "Revenue Recovered" dashboard metric ships as designed** — total value of renewals that occurred after a reminder was sent.

### Rationale

Small gym owners judge the product on the size of the number they see. A conservatively-framed metric risks looking unimpressive early, when the gym has not yet accumulated enough renewals for the figure to be compelling, and an unimpressive number at month one costs the subscription at month two.

### The known limitation

Some members in that figure would have renewed regardless — they were always going to walk in. The metric therefore measures *renewals following a reminder*, not *renewals caused by a reminder*. The two are not the same number, and the gap is unknown.

This also means §31's "incremental revenue attributable to the product" is not currently measurable — *incremental* means "would not have happened otherwise," which requires a comparison the product does not make.

### Revisit plan

Raise it with pilot gyms directly: ask whether they want to see WhatsApp-driven renewals separated from ordinary renewals. If they do, the clean way to measure it is a holdback:

> In one pilot gym, withhold reminders from a random 10% of expiring members for one month. Compare their renewal rate against the 90% who received reminders. The difference is the true incremental lift.

Run once, that produces a defensible figure ("reminders lift renewals by N points") usable in every future sales conversation. Deferred until pilot feedback indicates whether customers want it.

---

## 31. Key Success Metric

The most important early metric:

> **Incremental renewals/revenue attributable to the product.**

See §30 — measuring this properly requires the holdback experiment, which is deferred.

Supporting metrics:

- Gym activation
- Weekly active gyms
- Members managed
- WhatsApp messages delivered
- Renewal conversion
- Inactive member reactivation
- Monthly recurring revenue
- Customer churn

---

## 32. First Version Success Criteria

The MVP is ready for pilot when a gym can:

1. Create an account
2. Create a gym
3. Add/import members
4. Create membership plans
5. Track membership expiry
6. Extend a membership or add complimentary days
7. Track payments
8. Track attendance
9. Configure expiry reminders
10. Have reminders dispatched automatically on the correct dates
11. Track delivery status
12. See members requiring attention
13. Track renewals
14. See revenue at risk
15. See revenue recovered
16. Export basic reports

Items 10 and 11 are satisfied against the mock provider for MVP; live WhatsApp is P2 (§16.2).

---

# Part VI — Execution

## 33. Development Roadmap

> **Task-level detail lives in [EXECUTION.md](EXECUTION.md).** That file holds the numbered tasks, status tracking, exit criteria, open decisions, and risk register. This section gives the shape only — the two must not duplicate each other.

| Phase | Goal | Exit criteria |
|---|---|---|
| 0 — Validation | Confirm gym owners want this | 15–20 interviews; freeze decision made |
| 1 — Foundation | Safe multi-tenant skeleton | Tenant isolation provably works |
| 2 — Core Domain | Members, plans, expiry | Expiry correct under adjustment |
| 3 — Operations | Payments, attendance, dashboard | Owner can answer "where am I losing money?" |
| 4 — Automation | The differentiator | Right message, right member, right date |
| 5 — Growth Features | Leads, trials | Full funnel tracked |
| 6 — Pilot Readiness | Ship to real gyms | A gym onboardable in 30 minutes |
| 7 — Live WhatsApp | Replace the mock | Real messages delivered |

**Two things run in parallel rather than in sequence:**

- **Meta WhatsApp setup** starts in Phase 1, not Phase 7. Business verification and template approval have multi-day lead times (§16.2) and cost nothing to have approved and waiting. Starting late is the most likely cause of a blocked Phase 7.
- **Customer interviews** (Phase 0) continue through early build. They answer the freeze question (§6.5) and validate pricing (§24).

The two highest-risk areas are **tenant isolation** (Phase 1) and **expiry computation** (Phase 2). Both are cheap to get right up front and expensive to correct under live gym data.

---

## 34. Immediate Next Steps

1. Interview 15–20 gym owners
2. Identify the exact renewal/payment/attendance workflow they currently use
3. Ask the freeze questions in §6.5 — decide whether to build it, and to what policy
4. Begin Meta Business account verification (long lead time)
5. Create clickable UI wireframes
6. Define the database schema
7. Define the MVP API
8. Build the responsive web MVP
9. Build the expiry workflow first, against the mock provider
10. Get 5 pilot gyms
11. Measure actual renewal improvement and willingness to pay
12. Iterate based on real customer behavior
13. Only then expand into live WhatsApp, PT, campaigns, native apps, and AI

---

## 35. Decision Log

| # | Decision | Rationale | Section |
|---|---|---|---|
| D1 | Next.js full-stack; no separate NestJS backend for MVP | One codebase, one language; Auth.js works natively | §11.2 |
| D2 | Business logic in a service layer, not route handlers | Scan and API share one implementation; preserves extraction option | §11.3 |
| D3 | Redis and BullMQ deferred | A daily scan needs one cron, not a job queue | §11.2 |
| D4 | Vercel Cron for scheduling; single deploy target | No long-lived worker required at MVP scale | §11.4 |
| D5 | Shared-schema multi-tenancy, gym as tenant | Members do not log in; simplest model that fits | §13.1 |
| D6 | Prisma client extension enforces `gym_id` | A leaking query becomes impossible to write by accident | §13.2 |
| D7 | Daily scan, not pre-queued jobs | Adjustments change expiry; pre-queued messages go stale | §14.1 |
| D8 | All scheduling in IST; dates stored as dates | UTC midnight is 05:30 IST — wrong send time, wrong day boundary | §14.3 |
| D9 | Expiry computed additively; adjustments stored as records | Auditable, reversible, one rule for every adjustment type | §5.5 |
| D10 | Mock WhatsApp provider for MVP | No Meta account needed; scheduling logic is provider-independent | §16.1 |
| D11 | Every send persisted to `Notification`, not just logged | Gives delivery UI, dedupe, audit trail, testable assertions | §16.1 |
| D12 | "Revenue Recovered" ships as designed | Owner decision — small number early risks losing the subscription | §30 |
| D13 | Payment gateway is P2; MVP records payments only | Validate follow-up value before building collection | §5.7 |
| D14 | Membership freeze deferred to P1 | Mechanism is simple, policy is not — needs owner interviews first | §6.5 |
| D15 | Membership status derived, not stored | Status and date cannot disagree; new states need no backfill | §5.3 |

---

# Product North Star

## "Never lose a gym renewal because someone forgot to follow up."

The first version should be simple, reliable, WhatsApp-first, mobile-friendly, and focused on measurable business outcomes rather than trying to become a feature-heavy gym ERP.
