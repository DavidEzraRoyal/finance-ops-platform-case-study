# Operations & Finance Platform: a case study

> A production platform I designed and built as sole architect and engineer (with AI-assisted development) for a multi-company facility-services group with 1,800+ employees.
> The code and the data are proprietary and private. This repository documents the problem, the architecture and the engineering decisions. It contains no source code, client data or internal identifiers.

---

## The problem

The group ran on spreadsheets and accounting exports. Basic management questions had no defensible answer:

- **Which sites actually make money**, and where does margin leak?
- **What do receivables really look like** once accounting corrections are applied?
- **Do hours, payroll and the cost book agree?**
- **Which obligations are about to lapse:** licences, contracts, inspections?
- **What should a given manager do about it today?**

## What I built

| Module | What it answers |
|---|---|
| Site P&L | Revenue, direct cost and contribution per site, per month |
| Cost structure | Cost of sales vs. G&A, in three tiers: contribution drives operating decisions, full absorption drives reporting |
| Receivables | Ageing, collection risk and an arrears figure that survives scrutiny |
| Payroll | Three-way reconciliation of hours, payroll and the cost book |
| Cash | Budget vs. actual and a rolling 13-week cash-flow forecast |
| CRM | Customer accounts, leads, quotes and service tickets with SLA exceptions |
| Risk & compliance | Risk register with heatmap, licence expiry, contract coverage |
| Equipment | Asset register and a structured inspection programme |
| Improvement | Mapped opportunities with cost, annual value and ROI, on a monthly cycle |
| Tasks & knowledge | Tasks spawned automatically from exceptions; an internal knowledge base |

## By the numbers

| | |
|---|---|
| Codebase | ~64,000 lines (TypeScript, SQL, Python) |
| Application | 50 pages · 7 roles |
| Database | 28 versioned migrations; integrity enforced in the schema |
| Quality | 45 test suites · typecheck, lint, test and build in CI on every pull request |
| Delivery | 190+ commits, 35+ pull requests, from first commit to production in weeks |

## Architecture

```
Web app      Next.js (App Router, server components) · React · Tailwind · RTL-first
Data layer   PostgreSQL (Supabase) · Drizzle ORM · versioned migrations
ETL          Python loaders for accounting and payroll exports
Auth         Session at the edge; a central route registry decides access per role
CI/CD        GitHub Actions against a disposable Postgres · Vercel deployments
```

## Engineering principles

**A number that cannot be defended is not shown.** When the data covers only part of a figure, the page states the coverage and names the source that would complete it.

**Fail closed.** The access registry throws on an unclassified route instead of defaulting open. A test enumerates every page, so an unclassified page breaks the build.

**Decision numbers and reporting numbers are kept apart.** Contribution disappears when a site closes; G&A does not. Mixing the two is how service businesses close sites that were paying for head office.

**Provenance travels with the figure.** Numbers taken from a dated document carry that date, and it is shown beside the number.

**Integrity lives in the schema.** Foreign keys, CHECK constraints and unique business keys. A constraint cannot be forgotten by the next caller.

**Regulatory claims are cited or explicitly marked unverified.** A database constraint makes it impossible to add a regulatory checkpoint without declaring that status.

## A few real problems solved

- **Migrations that had never really run in production.** I found an unterminated comment in a baseline migration and an identity-sequence overflow caused by JavaScript float rounding (`9223372036854776000` vs `…775807`). I then applied the missing migrations from inside the production runtime without ever exposing the database credential.
- **Real accounting data that breaks naive models.** Negative monthly revenue from genuine corrections required changing the constraint, not clamping the data.
- **Duplicate master data.** Fixed at the business key in the schema, not patched in the UI.

## My role

Sole architect and engineer, working with AI coding agents under my review, and also the finance lead who defined what every number means. I gathered requirements with the CEO, finance and operations, then designed the data model, built, tested, deployed and trained the users.

---

**David Ezra** · Finance & technology · [Profile](https://github.com/DavidEzraRoyal)
