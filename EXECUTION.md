# Execution Plan

Working document for building the gym micro-SaaS. Tracks **what to do, in what order, and whether it is done.**

- **Why** anything is built this way → [gym-micro-saas-business-plan.md](gym-micro-saas-business-plan.md)
- **What** to build and when → this file

Section references like `§13.2` point at the plan. This file does not restate rationale — if a task looks arbitrary, the plan section explains it.

**Status key:** `⬜ not started` · `🟡 in progress` · `✅ done` · `⏸️ blocked`

---

## Current State

| | |
|---|---|
| **Phase** | 0 — Validation (not yet started) |
| **Sprint** | — |
| **Blocked on** | Nothing |
| **Next action** | Begin gym owner interviews (§27) |

Update this block at the start of each working session.

---

## Phase Overview

| Phase | Goal | Exit criteria |
|---|---|---|
| [0 — Validation](#phase-0--validation) | Confirm gym owners want this | 15–20 interviews done; freeze decision made |
| [1 — Foundation](#phase-1--foundation) | Safe multi-tenant skeleton | Tenant isolation provably works |
| [2 — Core Domain](#phase-2--core-domain) | Members, plans, expiry | Expiry correct under adjustment |
| [3 — Operations](#phase-3--operations) | Payments, attendance, dashboard | Owner can answer "where am I losing money?" |
| [4 — Automation](#phase-4--automation) | The differentiator | Right message, right member, right date |
| [5 — Growth Features](#phase-5--growth-features) | Leads, trials | Full funnel tracked |
| [6 — Pilot Readiness](#phase-6--pilot-readiness) | Ship to real gyms | A gym can be onboarded in 30 min |
| [7 — Live WhatsApp](#phase-7--live-whatsapp) | Replace the mock | Real messages delivered |

Phases 1–6 map to the plan's Sprints 1–8 (§33). Phase 0 precedes them; Phase 7 follows.

---

## Parallel Track — Meta WhatsApp Setup

**Start during Phase 1. Do not wait for Phase 7.**

Meta prerequisites have multi-day lead times and cost nothing to have approved and waiting (§16.2). Starting this late is the single most likely cause of a blocked Phase 7.

| # | Task | Status | Notes |
|---|---|---|---|
| M1 | Create Meta Business account | ⬜ | |
| M2 | Business verification (document checks) | ⬜ | Multi-day |
| M3 | Acquire WhatsApp Business phone number | ⬜ | Cannot be a number already on consumer WhatsApp |
| M4 | Draft the ~10 message templates | ⬜ | Copy from §5.6 / §5.7 |
| M5 | Submit templates to Meta | ⬜ | Each reviewed separately |
| M6 | Handle rejections, resubmit | ⬜ | Expect some; usually promotional-vs-utility miscategorisation |
| M7 | Confirm per-message pricing | ⬜ | Feeds §24 pricing |

---

## Phase 0 — Validation

**Goal:** confirm the problem is real and worth paying for, before writing production code.

**Plan reference:** §27, §34

### Tasks

| # | Task | Status | Done when |
|---|---|---|---|
| 0.1 | Interview 15–20 gym owners | ⬜ | 15 completed, notes written up |
| 0.2 | Ask the 15 standard questions (§27) | ⬜ | Asked in every interview |
| 0.3 | Ask the 8 freeze questions (§6.5) | ⬜ | Asked in every interview |
| 0.4 | Document current renewal/payment/attendance workflow | ⬜ | Written per gym |
| 0.5 | Record willingness to pay | ⬜ | A number per gym |
| 0.6 | Decide: build freeze, and to what policy | ⬜ | Decision recorded in plan §6.5 |
| 0.7 | Validate pricing hypothesis (§24) | ⬜ | Tiers confirmed or revised |
| 0.8 | Line up 5 pilot gyms | ⬜ | 5 verbal commitments |
| 0.9 | Clickable UI wireframes | ⬜ | Core screens covered |

### Exit criteria

- [ ] 15+ interviews completed
- [ ] Freeze decision made and recorded
- [ ] Pricing validated against real conversations
- [ ] 5 pilot gyms identified
- [ ] At least one owner has said they would pay

> **Do not lead the interview toward confirming the product idea** (§27). An interview that only produces agreement has produced nothing.

---

## Phase 1 — Foundation

**Goal:** a multi-tenant skeleton where cross-tenant data leaks are structurally impossible.

**Plan reference:** §11, §13, §33 Sprint 1

### 1a — Project setup

| # | Task | Status |
|---|---|---|
| 1.1 | Next.js (App Router) + TypeScript | ⬜ |
| 1.2 | Tailwind + shadcn/ui | ⬜ |
| 1.3 | Service-layer directory structure (§11.3) | ⬜ |
| 1.4 | ESLint, Prettier, strict `tsconfig` | ⬜ |
| 1.5 | Vitest + Playwright configured | ⬜ |
| 1.6 | GitHub Actions: lint, typecheck, test on PR | ⬜ |
| 1.7 | Vercel project + managed Postgres | ⬜ |
| 1.8 | Environment variable handling; no secrets in source (§17.1) | ⬜ |

### 1b — Data layer

| # | Task | Status |
|---|---|---|
| 1.9 | Prisma installed, connected | ⬜ |
| 1.10 | Initial schema: Gym, User, Role (§13.1) | ⬜ |
| 1.11 | `gym_id` on every gym-owned table | ⬜ |
| 1.12 | Composite indexes leading with `gym_id` | ⬜ |
| 1.13 | First migration applied | ⬜ |

### 1c — Tenant isolation ⚠️

The highest-risk area in the product. A single missed filter exposes one gym's members to another (§13.2).

| # | Task | Status |
|---|---|---|
| 1.14 | Request-scoped tenant context | ⬜ |
| 1.15 | Prisma client extension auto-injecting `gym_id` | ⬜ |
| 1.16 | Extension covers findMany / findUnique / update / delete / count / aggregate | ⬜ |
| 1.17 | Writes stamp `gym_id` automatically | ⬜ |
| 1.18 | Queries outside a tenant context fail loudly rather than returning everything | ⬜ |
| 1.19 | **Dedicated tenant isolation test suite** (§18.2) | ⬜ |

### 1d — Auth

| # | Task | Status |
|---|---|---|
| 1.20 | Auth.js configured | ⬜ |
| 1.21 | **Decide: database sessions vs JWT** — see [Open Decisions](#open-decisions) | ⬜ |
| 1.22 | Signup → creates Gym + Owner user | ⬜ |
| 1.23 | Login, logout, password reset | ⬜ |
| 1.24 | Password hashing | ⬜ |
| 1.25 | Owner / Staff roles and route protection | ⬜ |
| 1.26 | Session carries `gym_id` into tenant context | ⬜ |

### Exit criteria

- [ ] Owner can sign up, create a gym, log in and out
- [ ] **A query in Gym A's context cannot return Gym B's rows, and there is a test proving it**
- [ ] CI green on every PR
- [ ] Deployed to Vercel
- [ ] Meta setup (M1–M3) started in parallel

---

## Phase 2 — Core Domain

**Goal:** members, plans, and an expiry date that stays correct when adjusted.

**Plan reference:** §5.3, §5.4, §5.5, §33 Sprint 2

### 2a — Plans

| # | Task | Status |
|---|---|---|
| 2.1 | MembershipPlan CRUD | ⬜ |
| 2.2 | Name, duration, price | ⬜ |
| 2.3 | Joining fee, discount, GST | ⬜ |
| 2.4 | Extension and complimentary day settings | ⬜ |

### 2b — Members

| # | Task | Status |
|---|---|---|
| 2.5 | Member CRUD | ⬜ |
| 2.6 | Required fields: name, phone | ⬜ |
| 2.7 | Optional fields per §5.3 | ⬜ |
| 2.8 | Phone validation and per-gym uniqueness | ⬜ |
| 2.9 | Member search | ⬜ |
| 2.10 | Member profile screen | ⬜ |

### 2c — Membership & expiry ⚠️

The other high-risk area. Get this wrong and every reminder in the product is wrong.

| # | Task | Status |
|---|---|---|
| 2.11 | Membership created from plan; base expiry derived from duration | ⬜ |
| 2.12 | `MembershipAdjustment` table (type, days, reason, applied_by) | ⬜ |
| 2.13 | `effectiveExpiry()` = base + Σ adjustments (§5.5) | ⬜ |
| 2.14 | Apply extension / complimentary days | ⬜ |
| 2.15 | Remove an adjustment; expiry recomputes | ⬜ |
| 2.16 | Status derived from effective expiry, never stored (§5.3) | ⬜ |
| 2.17 | Dates stored as dates, not timestamps (§14.3) | ⬜ |
| 2.18 | Renewal extends the membership | ⬜ |
| 2.19 | **Unit tests per §18.1** | ⬜ |

### Exit criteria

- [ ] Owner can create plans and add members
- [ ] Expiry is computed, never stored as a mutated value
- [ ] Adding and removing an adjustment both produce the correct date
- [ ] Status always agrees with effective expiry
- [ ] IST boundary tests pass at 00:00 and 23:59

---

## Phase 3 — Operations

**Goal:** the owner can see where money is being lost today.

**Plan reference:** §5.2, §5.7, §5.8, §5.9, §33 Sprints 3–4

### 3a — Payments

| # | Task | Status |
|---|---|---|
| 3.1 | Payment records against a membership | ⬜ |
| 3.2 | Amount due / paid / outstanding | ⬜ |
| 3.3 | Due date and overdue derivation | ⬜ |
| 3.4 | Partial payments | ⬜ |
| 3.5 | Payment history on member profile | ⬜ |

> No gateway in MVP — records only (§5.7, D13).

### 3b — Attendance

| # | Task | Status |
|---|---|---|
| 3.6 | Manual check-in by member search | ⬜ |
| 3.7 | Attendance history | ⬜ |
| 3.8 | Last-visit date on profile | ⬜ |
| 3.9 | Check-in blocked for expired memberships | ⬜ |
| 3.10 | Inactivity calculation: 7 / 14 / 30 days | ⬜ |

### 3c — Dashboard

| # | Task | Status |
|---|---|---|
| 3.11 | Counts: members, active, expiring, expired, due, inactive | ⬜ |
| 3.12 | This month: new members, renewals, revenue | ⬜ |
| 3.13 | **Revenue at Risk** (§5.2) | ⬜ |
| 3.14 | Action Required panel | ⬜ |
| 3.15 | Each figure links to its filtered list | ⬜ |
| 3.16 | Mobile layout works at 360px | ⬜ |

### Exit criteria

- [ ] Dashboard answers "where am I losing money today?"
- [ ] Every number is clickable through to the underlying members
- [ ] Revenue at Risk computes correctly
- [ ] Usable on a phone browser

---

## Phase 4 — Automation

**Goal:** the differentiator. The right message reaches the right member on the right date, with no human involved.

**Plan reference:** §14, §15, §16, §33 Sprints 5–6

### 4a — Provider abstraction

| # | Task | Status |
|---|---|---|
| 4.1 | `WhatsAppProvider` interface (§16) | ⬜ |
| 4.2 | `MockWhatsAppProvider` — logs payload, returns success | ⬜ |
| 4.3 | Provider selected by environment variable | ⬜ |
| 4.4 | `Notification` table: member, template, scheduled, sent, status, response | ⬜ |
| 4.5 | **Every attempt persisted, not only logged** (§16.1, D11) | ⬜ |

### 4b — Templates & engine

| # | Task | Status |
|---|---|---|
| 4.6 | `NotificationTemplate` with variable substitution | ⬜ |
| 4.7 | Seed the templates from §5.6 and §5.7 | ⬜ |
| 4.8 | Trigger / condition / action engine (§15) | ⬜ |
| 4.9 | **Messages are data, not branches** — a new reminder is a row, not a deploy | ⬜ |
| 4.10 | Per-gym enable/disable per automation | ⬜ |

### 4c — Daily scan

| # | Task | Status |
|---|---|---|
| 4.11 | `/api/cron/daily-scan` route | ⬜ |
| 4.12 | Vercel Cron at 09:00 IST (§14.3) | ⬜ |
| 4.13 | Route authenticated — not publicly triggerable | ⬜ |
| 4.14 | Expiry reminders at 30/15/7/3/1 days | ⬜ |
| 4.15 | Post-expiry message | ⬜ |
| 4.16 | Payment due and overdue reminders | ⬜ |
| 4.17 | Inactivity at 7/14/30 days | ⬜ |
| 4.18 | **Idempotency guard — running twice sends nothing twice** (§14.4) | ⬜ |
| 4.19 | Scan reads `effectiveExpiry`, never base expiry | ⬜ |
| 4.20 | A WhatsApp failure does not abort the scan (§17.2) | ⬜ |
| 4.21 | Scan run logged: started, finished, counts, failures | ⬜ |

### 4d — Visibility & demo

| # | Task | Status |
|---|---|---|
| 4.22 | Delivery status UI | ⬜ |
| 4.23 | Failed message list with retry | ⬜ |
| 4.24 | **"Simulate Expiry" button** (§20) | ⬜ |
| 4.25 | Welcome message on member creation | ⬜ |

### 4e — Tests

| # | Task | Status |
|---|---|---|
| 4.26 | Injectable clock — no real waiting in tests | ⬜ |
| 4.27 | Scan against seeded DB produces exactly the expected rows | ⬜ |
| 4.28 | Running the scan twice produces no duplicates | ⬜ |
| 4.29 | Adjusting expiry shifts reminders to the new date | ⬜ |
| 4.30 | IST boundary correctness | ⬜ |

### Exit criteria

- [ ] Daily scan runs on schedule and is idempotent
- [ ] Reminders fire on exactly the right dates, verified by test
- [ ] Every attempt is visible in the UI
- [ ] Simulate Expiry demonstrates the full loop in under a minute
- [ ] A provider failure never breaks the scan

> **Observability matters here more than anywhere else.** A scan that silently stops running breaks nothing visible — the product simply stops working while appearing fine (§17.2).

---

## Phase 5 — Growth Features

**Goal:** the funnel above membership — leads and trials.

**Plan reference:** §6.1, §6.2, §6.3, §33 Sprint 7

| # | Task | Status |
|---|---|---|
| 5.1 | Lead CRUD with the §6.1 fields | ⬜ |
| 5.2 | Status pipeline: New → Contacted → Trial → Joined → Lost | ⬜ |
| 5.3 | Follow-up date and staff assignment | ⬜ |
| 5.4 | Today's follow-ups on dashboard | ⬜ |
| 5.5 | Convert lead → member | ⬜ |
| 5.6 | Funnel view and conversion rate | ⬜ |
| 5.7 | Trial start/end, type, interested plan | ⬜ |
| 5.8 | Trial-ending reminder via the scan | ⬜ |
| 5.9 | Trial → membership conversion | ⬜ |
| 5.10 | Revenue Recovery dashboard (§6.4) | ⬜ |

### Exit criteria

- [ ] Leads tracked end to end with conversion rate
- [ ] Trials remind before ending
- [ ] No lead silently goes cold

---

## Phase 6 — Pilot Readiness

**Goal:** a real gym can be onboarded and a salesman can demo in five minutes.

**Plan reference:** §5.10, §5.11, §21, §25, §33 Sprint 8

### 6a — Import

| # | Task | Status |
|---|---|---|
| 6.1 | CSV/Excel upload, columns per §5.11 | ⬜ |
| 6.2 | Preview before commit | ⬜ |
| 6.3 | Validation errors per row | ⬜ |
| 6.4 | Duplicate detection | ⬜ |
| 6.5 | Import summary | ⬜ |
| 6.6 | Partial import — valid rows land, invalid are reported | ⬜ |

### 6b — Reports

| # | Task | Status |
|---|---|---|
| 6.7 | The ten MVP reports (§5.10) | ⬜ |
| 6.8 | Date range filters | ⬜ |
| 6.9 | CSV export | ⬜ |
| 6.10 | Excel export | ⬜ |

### 6c — Production readiness

| # | Task | Status |
|---|---|---|
| 6.11 | Audit log for important actions (§17.1) | ⬜ |
| 6.12 | Sentry wired up | ⬜ |
| 6.13 | PostHog wired up | ⬜ |
| 6.14 | Error states and empty states throughout | ⬜ |
| 6.15 | **Alert if the daily scan fails or does not run** | ⬜ |
| 6.16 | Database backups confirmed | ⬜ |

### 6d — Demo & onboarding

| # | Task | Status |
|---|---|---|
| 6.17 | Demo gym seed data (§21) | ⬜ |
| 6.18 | Scenarios A–E all reachable | ⬜ |
| 6.19 | Onboarding checklist UI (§25) | ⬜ |
| 6.20 | Rehearse: gym operational in 15–30 minutes | ⬜ |
| 6.21 | Rehearse: full demo in under 5 minutes | ⬜ |

### 6e — E2E

| # | Task | Status |
|---|---|---|
| 6.22 | Signup → gym → plans → import → dashboard | ⬜ |
| 6.23 | Add member → welcome notification row | ⬜ |
| 6.24 | Advance clock → correct reminders | ⬜ |
| 6.25 | Extend membership → reminders shift | ⬜ |
| 6.26 | All five demo scenarios | ⬜ |

### Exit criteria

- [ ] Every item in §32 First Version Success Criteria satisfied
- [ ] A gym's Excel file imports cleanly
- [ ] Demo rehearsed end to end in under 5 minutes
- [ ] Monitoring live, scan failure alerts working
- [ ] **Ready for first pilot gym**

---

## Phase 7 — Live WhatsApp

**Goal:** swap the mock for Meta. Should be configuration, not a rewrite — that is the point of §16.

**Prerequisite:** the parallel Meta track (M1–M7) complete.

| # | Task | Status |
|---|---|---|
| 7.1 | `MetaCloudWhatsAppProvider` against the same interface | ⬜ |
| 7.2 | Template key mapping: internal → Meta-approved | ⬜ |
| 7.3 | Delivery status webhook | ⬜ |
| 7.4 | Webhook signature verification | ⬜ |
| 7.5 | Webhook idempotency | ⬜ |
| 7.6 | Retry with backoff on transient failure | ⬜ |
| 7.7 | Per-gym message quota and cost tracking | ⬜ |
| 7.8 | Opt-out handling | ⬜ |
| 7.9 | Flip the environment variable in staging | ⬜ |
| 7.10 | Send real messages to internal test numbers | ⬜ |
| 7.11 | Enable for the first pilot gym | ⬜ |

### Exit criteria

- [ ] Real messages delivered to real members
- [ ] Delivery status reflected in the UI
- [ ] Costs tracked per gym
- [ ] **No business logic changed to make this work** — if it did, §16's abstraction failed and should be corrected

---

## Post-Launch — Pilot Operations

**Plan reference:** §28, §30

| # | Task | Status |
|---|---|---|
| P.1 | Onboard 5–10 pilot gyms | ⬜ |
| P.2 | Interview each every 1–2 weeks | ⬜ |
| P.3 | Track the §28 measures per gym | ⬜ |
| P.4 | Ask whether they want WhatsApp-driven renewals shown separately (§30) | ⬜ |
| P.5 | If yes: run the 10% holdback experiment in one gym | ⬜ |
| P.6 | Validate pricing against real usage | ⬜ |
| P.7 | Decide on freeze based on Phase 0 answers | ⬜ |

---

## Open Decisions

Resolve before the phase that needs them. Record the outcome in the plan's §35 decision log.

| # | Decision | Needed by | Notes |
|---|---|---|---|
| OD1 | Auth.js: database sessions vs JWT | Phase 1 | DB sessions allow instant revocation and are simpler to reason about; JWT avoids a lookup per request. At this scale the lookup is free — DB sessions are the likely answer |
| OD2 | Build freeze at all, and to what policy | Phase 0 exit | Answered by interviews (§6.5) |
| OD3 | Soft delete vs hard delete for members | Phase 2 | Gyms will want to see former members; soft delete is likely |
| OD4 | Multi-branch gyms — one gym or many | Phase 2 | Affects the tenant model. §8.3 defers branches, but decide now whether the schema forecloses it |
| OD5 | Phone number as the member identity key | Phase 2 | Affects duplicate detection on import |
| OD6 | Staff permissions granularity | Phase 1 | Owner/Staff may be enough (§5.1); confirm against interviews |

---

## Risk Register

| Risk | Impact | Mitigation | Phase |
|---|---|---|---|
| Cross-tenant data leak | **Severe** — kills trust permanently | Prisma extension + dedicated test suite (1.14–1.19) | 1 |
| Daily scan silently stops | **Severe** — product stops working, nothing looks broken | Run logging + failure alerting (4.21, 6.15) | 4, 6 |
| Wrong reminder dates | High — actively embarrassing to the gym | Injectable clock, IST tests, adjustment-shift tests (4.26–4.30) | 4 |
| Meta approval blocks launch | High | Parallel track from Phase 1 (M1–M7) | 1 |
| Template rejected repeatedly | Medium | Submit early, mark utility not marketing | Parallel |
| Import fails on real gym data | Medium | Test with actual gym Excel files, not synthetic ones | 6 |
| Duplicate messages annoy members | Medium | Idempotency guard (4.18) | 4 |
| Scan outgrows function timeout | Low at pilot scale | Trigger condition documented (§11.2); add BullMQ | Later |

---

## Working Agreements

- **Tests accompany the risky parts.** Tenant isolation, expiry computation, and scan scheduling are not optional to test — they are where the product's credibility lives.
- **Business logic goes in services, not route handlers** (§11.3). The scan and the API must call the same code.
- **Never read `base_expiry` outside the computation function.** Everything downstream uses `effectiveExpiry()`.
- **No secrets in source** (§17.1).
- **Every feature passes the §10 test:** does it get more members, keep members, get paid faster, reduce manual work, or explain the business? If not, it does not belong in the MVP.
- **When a decision is made, record it** in the plan's §35 — not only here.
