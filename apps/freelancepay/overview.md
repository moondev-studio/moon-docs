# FreelancePay — Product Overview

> Version: 0.1.0 | Updated: 2026-04-10 | Status: Design (planned)
> Phase: Scaffolding — design-first spec

## Product Concept

| Item | Detail |
|------|--------|
| Slogan | "The simplest invoicing tool for freelancers" |
| Target | Solo freelancers and micro-teams (1-5 people) worldwide |
| Platform | Android, iOS, Web (primary) |
| Bundle ID | com.moondeveloper.freelancepay |
| Tech Stack | KMP (Compose Multiplatform), MVI, Koin, Ktor 3.x |

## Problem Statement

Freelancers juggle multiple clients, currencies, and tax rules. Existing tools are either too complex (QuickBooks) or too simple (spreadsheets). FreelancePay fills the gap with:
- One-tap invoice generation with PDF export
- Per-client revenue tracking across currencies
- Automated tax estimation based on locale
- Overdue payment alerts so nothing slips through

## Target Users

- Solo freelancers (designers, developers, writers, consultants)
- Micro-agencies (1-5 members) who invoice multiple clients
- Digital nomads needing multi-currency support

## MVP Scope

1. **Client Management** — CRUD for clients with contact info, default currency, payment terms
2. **Invoice Creation** — Line items, tax, discounts, due dates; PDF export
3. **Payment Tracking** — Mark invoices paid/partial/overdue; payment history
4. **Revenue Statistics** — Monthly revenue chart, per-client breakdown
5. **Tax Estimation** — Locale-aware tax rate suggestions via `moon-i18n-kmp`
6. **Overdue Alerts** — Push notifications for past-due invoices
7. **Time Tracking** — Reuse FocusOn timer core for hourly billing

## Revenue Model

| Tier | Price | Limits |
|------|-------|--------|
| Free | $0 | 3 active clients, 5 invoices/month, basic stats |
| Pro Monthly | $4.99/mo (KRW 4,900) | Unlimited clients/invoices, PDF templates, tax estimation, priority support |
| Pro Annual | $39.99/yr (KRW 39,900) | Same as Pro Monthly, ~33% savings |

**Primary revenue model: Pro subscription.** Free tier serves as acquisition funnel.

## Module Reuse

| Module | Reuse | Notes |
|--------|-------|-------|
| FocusOn Timer Core | ~85% | Remove pomodoro, keep pure time measurement |
| Splitly Settlement UI | ~60% | Adapt payment tracking patterns |
| moon-billing-kmp | 100% | Subscription / paywall |
| moon-i18n-kmp | 100% | Currency formatting, tax rates |
| moon-auth-kmp | 100% | Authentication |

## Key Technical Decisions

- Timer: FocusOn core reuse (pomodoro stripped) — see ADR-001
- Invoice PDF: Decision pending (client vs server) — see ADR-002
- Multi-currency: `moon-i18n-kmp` module
- Web primary: Firebase Hosting + SPA

## Next Steps

- Finalize feature spec and screen designs
- Implement domain layer (`FreelanceDomain`)
- Build server endpoints `/api/freelancepay/v1/*`
- PDF generation template design
