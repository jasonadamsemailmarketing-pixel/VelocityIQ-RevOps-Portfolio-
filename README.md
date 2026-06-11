# Intelligent Speed-To-Lead Engine 2.0
### A Full-Funnel RevOps Automation System
**Built by Jason Osarebo Adams — CRM Automation & Email Marketing Specialist**

[![Portfolio](https://img.shields.io/badge/Portfolio-jasonadams--portfolio.vercel.app-blue)](https://jasonadams-portfolio.vercel.app)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-jasonadams--email-blue)](https://linkedin.com/in/jasonadams-email)

---

## Overview

This project demonstrates the design and implementation of a production-grade, RevOps-informed Speed-To-Lead automation system built entirely on free-tier tools. Purpose-built for **VelocityIQ** — a fictional B2B SaaS revenue intelligence company.

Unlike conventional Speed-To-Lead systems that simply notify a rep when a form is submitted, this engine **enriches, scores, grades, routes, notifies, and enforces SLA accountability** — all before a human touches the lead.

| Scenarios | Modules | Tools | Cost |
|-----------|---------|-------|------|
| 2 | 26 | 6 | 100% Free Tier |

---

## The Problem This Solves

B2B SaaS companies lose up to 80% of inbound leads because reps take too long to respond. Research shows that responding within 5 minutes increases conversion by 100x compared to responding at 30 minutes. Most teams lack the automation infrastructure to enforce this. This system solves that problem end to end.

---

## Technology Stack

| Tool | Role |
|------|------|
| **Tally.so** | Lead capture form with webhook integration |
| **Make.com** | Automation orchestration — all logic, routing, scoring, and integrations |
| **HubSpot CRM** | CRM backbone — contacts, custom properties, deals, tasks, activity timeline |
| **Airtable** | Database layer — rep roster, territory routing rules, SLA tracker |
| **Slack** | Rep notifications and manager escalation alerts |
| **Google Sheets** | Lead enrichment lookup and response-time reporting log |

---

## System Architecture

### Scenario 1 — Main Lead Engine (18 Modules)
Fires the moment a prospect submits the demo request form. Processes the lead through enrichment, scoring, routing, notification, task creation, SLA logging, and response tracking — all within seconds.

### Scenario 2 — SLA Escalation Monitor (8 Modules)
Runs on a 15-minute schedule. Continuously monitors the SLA Tracker for leads that have breached the 5-minute response window and fires escalation alerts to the manager channel in Slack.

---

## Full System Flow

| Stage | What Happens |
|-------|-------------|
| 1. Capture | Lead submits Tally.so form. Make.com webhook fires instantly. |
| 2. Enrich | Email domain extracted. Google Sheets lookup returns company intelligence. |
| 3. Deduplicate | HubSpot searched for existing contact. Create or Update path selected. |
| 4. Score | Lead scored across company size, industry fit, job seniority, and intent signals. |
| 5. Grade | ICP Grade assigned: A (Strong), B (Moderate), or C (Weak). |
| 6. Route | Territory matched via Airtable Routing Rules. Rep assigned by round-robin load balancing. |
| 7. Notify | Rep receives rich Slack card with full lead context and HubSpot link. |
| 8. Task | HubSpot task created with 5-minute due date for the assigned rep. |
| 9. Track | SLA record logged in Airtable. Lead Response row written to Google Sheets. |
| 10. Escalate | Scheduled monitor fires breach alerts and updates statuses across all tools. |

---

## Lead Scoring & ICP Grading Model

Every lead is scored in real time before a rep sees it. Scores are calculated inline in Make.com using nested `if()` formulas — no external scoring tool required.

| Signal | Max Points | Logic |
|--------|-----------|-------|
| Company Size | 30 | Pulled from Google Sheets enrichment lookup |
| Industry Fit | 25 | SaaS/Tech = 25, Finance = 22, Healthcare = 20, Manufacturing = 18, Other = 10 |
| Job Title Seniority | 25 | C-Suite/Founder = 25, VP = 22, Director = 18, Manager = 14, Senior = 10 |
| Intent & Timeline | 20 | Immediately = 20, Within 1 month = 16, 1–3 months = 12, 3–6 months = 6 |

### ICP Grade Thresholds

| Grade | Score Range | Priority |
|-------|------------|---------|
| A — Strong ICP Fit | 65–80 pts | High priority. Rep must respond within 5 minutes. |
| B — Moderate ICP Fit | 40–64 pts | Qualified lead. Standard 5-minute SLA applies. |
| C — Weak ICP Fit | 0–39 pts | Low priority. Still processed but lower urgency. |

---

## Rep Routing Logic

### Step 1 — Territory Matching
Lead industry matched against Airtable Routing Rules table.

| Industry | Territory |
|----------|-----------|
| SaaS / Technology | North America |
| Finance / Insurance | EMEA |
| Healthcare / Pharma | APAC |
| Manufacturing / Logistics | North America |
| Other / Unknown | Default Round Robin |

### Step 2 — Load-Balanced Assignment
Make.com queries the Airtable Rep Roster filtered by territory, sorted by Current Lead Count ascending — the rep with the fewest active leads always receives the next assignment.

| Rep | Territory |
|-----|-----------|
| Sarah Mitchell | North America — SaaS/Tech |
| David Okafor | EMEA — Finance/Insurance |
| Priya Sharma | APAC — Healthcare/Pharma |
| Marcus Webb | North America — Manufacturing |

---

## SLA Governance & Escalation

| Status | Trigger |
|--------|---------|
| Pending | Lead just created. Rep has 5 minutes to make contact. |
| Within SLA | Rep contacted the lead within the 5-minute window. |
| Breached | No contact after 5 minutes. Manager alerted automatically. |
| Escalated | No contact after 15 minutes. Senior management alerted. |

---

## Module Reference

### Scenario 1 — Main Lead Engine

| Module | Tool | Function |
|--------|------|---------|
| 1 | Webhooks | Custom webhook trigger — captures Tally.so submissions instantly |
| 3 | Google Sheets | Domain lookup for mock lead enrichment data |
| 4 | HubSpot CRM | Search for existing contact — duplicate detection |
| 5 | Router | Branches flow: New Contact vs Existing Contact |
| 6 | HubSpot CRM | Create a Contact — writes new lead to CRM |
| 7 | HubSpot CRM | Update a Contact — refreshes existing contact |
| 9 | Airtable | Matches industry to territory via Routing Rules table |
| 10 | Airtable | Fallback rule lookup when no industry match found |
| 11 | Airtable | Finds lowest-load rep in matched territory |
| 12 | HubSpot CRM | Writes lead score, ICP grade, enrichment, and rep assignment |
| 14 | Airtable | Increments assigned rep's lead count for round-robin |
| 15 | Slack | Rich rep alert posted to #velocityiq-new-leads |
| 16 | HubSpot CRM | Creates 5-minute due date task linked to contact |
| 17 | Airtable | Logs new SLA entry with Pending status and timestamp |
| 18 | Google Sheets | Appends full lead record to Lead Response Log |

### Scenario 2 — SLA Escalation Monitor

| Module | Tool | Function |
|--------|------|---------|
| 1 | Airtable | Finds Pending records older than 5 minutes |
| 2 | Airtable | Sets SLA Status to Breached |
| 3 | Slack | Manager alert fired to #velocityiq-sla-escalations |
| 4 | HubSpot CRM | Sets Lead Status to Breached |
| 5 | Airtable | Finds Breached records older than 15 minutes |
| 6 | Airtable | Sets SLA Status to Escalated |
| 7 | Slack | High-severity escalation alert fired to manager channel |
| 8 | Slack | Final escalation notification with elapsed time |

---

## RevOps Design Decisions

| Decision | What It Demonstrates |
|----------|---------------------|
| Duplicate detection before CRM write | Data hygiene discipline — CRM quality is foundational to pipeline trust |
| Mock enrichment with real architecture | Understands enrichment APIs (Apollo, Clearbit) — swappable on day one |
| Inline scoring without Set Variable modules | Deep Make.com formula proficiency — avoids anti-patterns that cause silent failures |
| Territory routing with load balancing | Thinks about scale — grows to a 20-rep team without reconfiguration |
| SLA governance as a second scenario | Understands speed-to-lead is about management accountability, not just rep notification |
| Response-time logging in Google Sheets | RevOps instinct — data must be captured for coaching, forecasting, and improvement |
| All tools on free tier | Resourceful — builds production-grade systems without budget |

---

## Production Notes

This system was built as a portfolio demonstration using free-tier tools and a fictional company. In a real production deployment:

- The mock Google Sheets enrichment lookup would be replaced by a live enrichment API such as Apollo.io, Clearbit, or People Data Labs. The Make.com module structure and all downstream references remain identical — only the data source changes.
- Slack User IDs would be real Member IDs enabling direct @mention notifications to reps.
- The Tally.so form would be embedded on a live product marketing page.

---

## About the Builder

**Jason Osarebo Adams**
CRM Automation & Email Marketing Specialist

- Email: jasonadams.emailmarketing@gmail.com
- LinkedIn: [linkedin.com/in/jasonadams-email](https://linkedin.com/in/jasonadams-email)
- Portfolio: [jasonadams-portfolio.vercel.app](https://jasonadams-portfolio.vercel.app)
