# Gym Micro-SaaS — Product & Business Plan

## 1. Product Vision

Build a lightweight gym-management SaaS focused on one core business outcome:

> **Help gym owners increase membership renewals, reduce missed payments, and bring inactive members back using automated WhatsApp follow-ups.**

The first version will be a responsive web application that works well on Android and iPhone browsers. Native Android/iOS apps can be considered later.

The product should not initially try to become a complete gym ERP. It should focus on the highest-value workflows around:

**Membership → Attendance → Expiry → WhatsApp → Renewal**

---

# 2. Target Customer

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

# 3. Problems to Solve

## Problem 1 — Membership Expiry

Gym owners often manually check which memberships are expiring.

### Solution

Automatically identify upcoming expiries and send WhatsApp reminders.

Example sequence:

- 30 days before expiry
- 15 days before expiry
- 7 days before expiry
- 3 days before expiry
- 1 day before expiry
- After expiry

---

## Problem 2 — Missed Payments

Members forget or delay payments.

### Solution

Track:

- Amount due
- Due date
- Paid amount
- Outstanding amount
- Overdue status

Send automated WhatsApp reminders.

---

## Problem 3 — Member Drop-Off

Members stop visiting but the gym may not notice until much later.

### Solution

Track attendance and identify members who have not visited for:

- 7 days
- 14 days
- 30 days

Send re-engagement messages and alert gym staff.

---

## Problem 4 — Lost Renewals

A member's membership expires and nobody follows up.

### Solution

Automated renewal campaigns before and after expiry.

---

## Problem 5 — Forgotten Leads

Gym enquiries from walk-ins, WhatsApp, phone calls, Instagram, etc. may not be followed up.

### Solution

Basic CRM:

- Lead creation
- Lead source
- Interested plan
- Lead status
- Follow-up date
- Notes
- Convert lead to member

Statuses:

`New → Contacted → Trial → Joined → Lost`

---

## Problem 6 — Trial Conversion

Free trials and short trials are easy to forget.

### Solution

Track trial dates and automatically follow up before the trial ends.

---

## Problem 7 — Poor Revenue Visibility

Gym owners may not know:

- How much revenue is due
- How many memberships are expiring
- How much revenue is at risk
- How many renewals happened
- How many members are inactive

### Solution

Simple business-focused dashboard.

---

# 4. Core Product Positioning

Do NOT primarily position the product as:

> "Gym Management Software"

Instead position it as:

> **"We help gyms increase membership renewals and reduce member drop-offs using automated WhatsApp follow-ups."**

Alternative positioning:

> **"Never lose a gym renewal because someone forgot to follow up."**

The product should demonstrate business outcomes rather than simply listing software features.

---

# 5. MVP Scope

The MVP should contain only the features necessary to validate the business.

## 5.1 Authentication

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

---

# 6. Dashboard

The dashboard should answer:

> "Where am I losing money today?"

Example:

```text
XYZ FITNESS

Members              487
Active               392
Expiring this week    18
Expired               31
Payment Due            14
Inactive               27

THIS MONTH

New Members            32
Renewals               41
Revenue           ₹2,84,000

ACTION REQUIRED

18 memberships expire this week
14 payments are overdue
27 members haven't visited in 14 days
8 leads haven't been contacted
```

## Important dashboard metric

### Revenue at Risk

Example:

```text
18 memberships expire in next 7 days

Average membership value: ₹4,000

Revenue at Risk: ₹72,000
```

CTA:

> Recover ₹72,000

This metric should connect the product directly to business value.

---

# 7. Member Management

Each member should have a profile.

Example:

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

Actions:

- WhatsApp
- Renew
- Payment
- Attendance
- Edit

Member fields should include:

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

---

# 8. Membership Plans

Gym owners can create plans.

Example:

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
- Freeze policy
- Extension
- Complimentary days

---

# 9. WhatsApp Automation

This should be the primary differentiator.

## Membership Messages

Automated messages:

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

---

# 10. Payment Notifications

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

Future integration candidates:

- Razorpay
- PhonePe
- Cashfree
- Stripe where appropriate

Do not build a custom payment processor.

---

# 11. Attendance

MVP:

- Manual check-in
- Member search
- Mark attendance
- View attendance history
- Last visit date

Future:

- QR attendance
- RFID
- Biometric integration
- Face recognition

Do not build biometric functionality in the initial MVP.

---

# 12. Inactivity Detection

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

---

# 13. Lead Management

Basic CRM.

Lead fields:

- Name
- Phone
- Source
- Interested plan
- Notes
- Assigned staff
- Follow-up date
- Status

Statuses:

```text
New
Contacted
Trial
Joined
Lost
```

Dashboard:

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

---

# 14. Lead Follow-Up

Example:

```text
Lead: Rahul Sharma
Status: Contacted
Last Contact: 2 days ago
Next Follow-Up: Today
```

Notification:

> Follow up with Rahul Sharma today.

Future automation:

- New lead WhatsApp acknowledgement
- Trial reminder
- Trial ending reminder
- Post-trial follow-up
- Lost lead reactivation

---

# 15. Trial Management

Support:

- Trial start date
- Trial end date
- Trial type
- Interested plan
- Conversion status

Example:

```text
Trial starts Monday

Monday:
Welcome to XYZ Fitness 💪

Friday:
Your trial ends tomorrow.
Would you like to continue with a membership?
```

---

# 16. PT / Trainer Management

Phase 2 feature.

Trainer:

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

---

# 17. Campaigns & Offers

Phase 2.

Allow owners to select audiences:

- Expired members
- Members expiring soon
- Inactive members
- Members in a particular age group
- PT members
- Former members
- Leads
- Specific membership plans

Example:

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

> 💪 We miss you!

> Come back this week and get 20% OFF your membership renewal.

Important:

- Respect WhatsApp Business/API policies.
- Use approved templates where required.
- Track message delivery/status.
- Do not implement spam-like behavior.

---

# 18. WhatsApp Conversational Sales

Future feature.

Example:

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

Potential future capabilities:

- Renew membership
- Check expiry
- Check payment due
- Book PT session
- Ask gym timings
- Ask membership prices
- Request support

This can eventually become an AI/automation layer.

---

# 19. Sales Demo Strategy

The salesman should NOT start by explaining every feature.

The demo should start with a business problem.

Example:

### Step 1

Ask:

> How many members do you have?

### Step 2

Ask:

> How many memberships expire every month?

### Step 3

Ask:

> How many of those members usually renew?

### Step 4

Show:

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

# 20. "Simulate Expiry" Demo Feature

Create a demo-friendly feature.

For a test member:

```text
Membership expires tomorrow
```

Button:

> Simulate 1 Day Before Expiry

System triggers the configured WhatsApp workflow.

This makes the product much easier for the salesman to demonstrate.

---

# 21. Revenue Recovery Dashboard

Track:

- Expiring memberships
- Renewal reminders sent
- Renewals completed
- Renewal rate
- Revenue recovered
- Revenue at risk
- Inactive members reactivated

Example:

```text
This Month

Memberships Expiring       50
Renewed                    32

Renewal Rate               64%

Revenue at Risk        ₹2,00,000
Revenue Recovered      ₹1,28,000
```

Do not claim that the SaaS caused revenue recovery unless attribution can actually be measured.

---

# 22. Reports

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

Export:

- CSV
- Excel

PDF can be added later.

---

# 23. Pricing Experiment

Initial pricing hypothesis:

## Starter — ₹999/month

- Up to 300 members
- Membership management
- Attendance
- Basic WhatsApp reminders

## Growth — ₹1,999/month

- Up to 1,000 members
- WhatsApp automation
- Leads
- Payment tracking
- Campaigns
- Reports

## Pro — ₹3,999/month

- Large member base
- Multiple trainers
- Advanced campaigns
- Payment integration
- Advanced analytics
- Multiple branches

These prices are hypotheses and should be validated through actual sales conversations.

WhatsApp/API message costs should be accounted for separately or included with reasonable usage limits.

---

# 24. Technology Strategy

## Initial Product

Use a responsive web application.

Goals:

- Desktop support
- Android browser support
- iPhone browser support
- Mobile-friendly UI
- PWA-ready architecture

Do not build native Android/iOS apps initially.

Native apps can be considered after product-market fit.

---

# 25. Suggested Architecture

```text
                    ┌─────────────────────┐
                    │ Responsive Web App  │
                    │ Desktop/Android/iOS │
                    └──────────┬──────────┘
                               │
                    ┌──────────▼──────────┐
                    │         API         │
                    └──────────┬──────────┘
                               │
              ┌────────────────┼────────────────┐
              │                │                │
       ┌──────▼──────┐  ┌─────▼─────┐  ┌──────▼──────┐
       │ PostgreSQL  │  │   Redis   │  │ Job Queue   │
       └─────────────┘  └───────────┘  └──────┬──────┘
                                              │
                                     ┌────────▼────────┐
                                     │ WhatsApp API    │
                                     └─────────────────┘
```

Potential backend technologies can be selected based on implementation speed and team expertise.

The architecture should support:

- Multi-tenancy
- Background jobs
- Scheduled notifications
- Audit logs
- Role-based access
- Secure API
- Payment integration
- WhatsApp provider abstraction

---

# 26. Multi-Tenant SaaS Requirement

The system should be designed from the beginning as a multi-tenant SaaS.

Core entities:

```text
Gym
User
Role
Member
MembershipPlan
Membership
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
```

Every gym-owned record must be associated with the appropriate gym/tenant.

---

# 27. Important Non-Functional Requirements

## Security

- Secure authentication
- Password hashing
- HTTPS
- Authorization checks
- Tenant isolation
- Audit logs for important actions
- Secure secrets management
- No API keys in source code

## Reliability

- Background jobs must be retryable
- WhatsApp failures should not crash core workflows
- Failed messages should be logged
- Payment webhooks must be idempotent
- Scheduled jobs must be observable

## Scalability

Initial scale target:

- 100 gyms
- 100,000 total members

Architecture should allow growth beyond this without major redesign.

---

# 28. Core Automation Engine

Build the notification system so new workflows can be added without hardcoding every message.

Example:

```text
Trigger:
membership.expiring

Condition:
days_until_expiry = 7

Action:
send_whatsapp_template(expiry_7_day)
```

Other triggers:

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

This automation engine should become a reusable core of the product.

---

# 29. WhatsApp Provider Abstraction

Do not tightly couple business logic to one WhatsApp provider.

Create an internal interface such as:

```text
WhatsAppProvider

sendTemplate()
sendMessage()
getMessageStatus()
```

This allows the business to change providers later.

Potential providers should be researched before implementation based on:

- Official WhatsApp Business API support
- Pricing
- Template support
- Delivery status
- Webhooks
- Indian business support
- Reliability
- Compliance

Do not rely on unofficial WhatsApp automation methods.

---

# 30. MVP User Journey

## Gym Owner

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

## Member

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

# 31. Data Import

Many gyms already have Excel data.

MVP should support:

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

Provide:

- Preview before import
- Validation errors
- Duplicate detection
- Import summary

Example:

```text
500 records uploaded

Valid: 472
Duplicates: 18
Invalid phone: 7
Missing expiry date: 3
```

This is important for onboarding existing gyms.

---

# 32. Onboarding

The salesman should be able to onboard a gym quickly.

Target:

> Gym should be operational within 15–30 minutes.

Onboarding checklist:

```text
✓ Gym created
✓ Plans created
✓ WhatsApp connected
✓ Members imported
✓ Templates configured
✓ First automation enabled
```

---

# 33. Product Analytics

Track product usage:

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

Important SaaS metrics:

- MRR
- ARR
- CAC
- LTV
- Churn
- Activation rate
- Retention
- Expansion revenue

---

# 34. Business Validation Strategy

Before building the complete product:

### Stage 1 — Customer Interviews

Talk to at least 15–20 gym owners.

Questions:

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

# 35. Pilot Strategy

Target:

> 5–10 gyms

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

# 36. MVP Development Priorities

## P0 — Must Have

- Multi-tenant authentication
- Gym setup
- Member CRUD
- Membership plans
- Membership expiry
- Payment tracking
- Attendance
- Dashboard
- WhatsApp integration
- WhatsApp templates
- Expiry automation
- Payment reminders
- Basic reports
- CSV/Excel import

## P1 — Important

- Lead CRM
- Trial management
- Inactivity automation
- Renewal analytics
- Revenue at risk
- Revenue recovered
- Campaigns

## P2 — Later

- PT management
- Trainer management
- QR attendance
- Payment links
- Advanced campaigns
- Multiple branches
- Member portal
- Native Android/iOS apps

## P3 — Future

- AI WhatsApp assistant
- Conversational sales
- AI support
- Workout plans
- Diet plans
- Biometric integrations
- RFID
- Face recognition

---

# 37. Things NOT to Build Initially

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

# 38. Sales Strategy

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

# 39. Competitive Differentiation

Potential differentiation:

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

Avoid competing solely on:

- Number of features
- Number of screens
- Complex enterprise functionality

---

# 40. Key Success Metric

The most important early metric should be:

> **Incremental renewals/revenue attributable to the product.**

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

# 41. First Version Success Criteria

The MVP can be considered ready for pilot when a gym can:

1. Create an account
2. Create a gym
3. Add/import members
4. Create membership plans
5. Track membership expiry
6. Track payments
7. Track attendance
8. Connect WhatsApp
9. Configure expiry reminders
10. Automatically send approved WhatsApp messages
11. Track delivery status
12. See members requiring attention
13. Track renewals
14. See revenue at risk
15. See revenue recovered
16. Export basic reports

---

# 42. Suggested Development Roadmap

## Sprint 1

Foundation:

- Project setup
- Authentication
- Multi-tenancy
- Database schema
- Gym setup
- User roles

## Sprint 2

Members:

- Member CRUD
- Membership plans
- Membership creation
- Expiry calculation
- Member profile

## Sprint 3

Payments + Attendance:

- Payment tracking
- Outstanding payments
- Manual attendance
- Attendance history

## Sprint 4

Dashboard:

- Active members
- Expiring members
- Expired members
- Payment due
- Inactive members
- Revenue at risk

## Sprint 5

WhatsApp:

- Provider integration
- Template management
- Message sending
- Delivery status
- Webhooks

## Sprint 6

Automation:

- Expiry reminders
- Payment reminders
- Welcome messages
- Expired-member messages
- Background jobs
- Retry handling

## Sprint 7

Leads + Trials:

- Lead management
- Follow-up reminders
- Trial tracking
- Conversion

## Sprint 8

Pilot readiness:

- CSV/Excel import
- Reports
- Audit logs
- Error handling
- Analytics
- Demo mode
- Salesman demo flow

---

# 43. Demo Data

Create a demo gym with realistic data.

Example:

```text
Gym:
XYZ Fitness

Members:
487

Active:
392

Expiring this week:
18

Expired:
31

Payment due:
14

Inactive:
27

Leads:
43

Trials:
12
```

Create several demo scenarios:

### Scenario A
Member expiring tomorrow.

### Scenario B
Payment overdue.

### Scenario C
Member inactive for 14 days.

### Scenario D
New lead requiring follow-up.

### Scenario E
Trial ending tomorrow.

The salesman should be able to demonstrate each scenario quickly.

---

# 44. Product Principle

Every feature should answer one of these questions:

### Does it help the gym:

1. Get more members?
2. Keep more members?
3. Get paid faster?
4. Reduce manual work?
5. Understand the business better?

If a feature does not meaningfully support one of these outcomes, it should probably not be part of the early MVP.

---

# 45. Long-Term Vision

The long-term product can evolve from:

```text
Gym Management
```

into:

```text
Gym Revenue & Engagement Platform
```

with:

```text
                    GYM
                     │
       ┌─────────────┼─────────────┐
       │             │             │
   MEMBERS        LEADS        PAYMENTS
       │             │             │
       └─────────────┼─────────────┘
                     │
                 AUTOMATION
                     │
               WhatsApp / SMS
                     │
              ┌──────▼──────┐
              │   Renewal   │
              │  Engagement │
              │    Sales    │
              └─────────────┘
```

The ultimate goal is to make the system proactive:

> Instead of the gym owner asking "What do I need to do today?", the product tells them exactly which members, leads, and payments need attention.

---

# 46. Immediate Next Steps

Before writing significant production code:

1. Interview 15–20 gym owners.
2. Identify the exact renewal/payment/attendance workflow they currently use.
3. Identify the WhatsApp provider/API that can legally and reliably support the required business messaging.
4. Create clickable UI wireframes.
5. Define the database schema.
6. Define the MVP API.
7. Build the responsive web MVP.
8. Build the WhatsApp expiry workflow first.
9. Get 5 pilot gyms.
10. Measure actual renewal improvement and willingness to pay.
11. Iterate based on real customer behavior.
12. Only then expand into PT, campaigns, native apps, and AI.

---

# Product North Star

## "Never lose a gym renewal because someone forgot to follow up."

The first version should be simple, reliable, WhatsApp-first, mobile-friendly, and focused on measurable business outcomes rather than trying to become a feature-heavy gym ERP.
