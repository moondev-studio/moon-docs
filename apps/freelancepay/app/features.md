# FreelancePay — Feature Spec

- Version: 0.1.0
- Date: 2026-04-10
- Status: All features **planned** (design-first, no implementation yet)

## Features

| ID | Area | Feature | Status | Notes |
|----|------|---------|--------|-------|
| F-001 | Client | Client CRUD (name, email, company, address) | planned | Core entity |
| F-002 | Client | Default currency per client | planned | Uses `moon-i18n-kmp` |
| F-003 | Client | Payment terms (net-15/30/45/60/custom) | planned | Drives overdue logic |
| F-004 | Invoice | Invoice creation (line items, qty, rate) | planned | Core feature |
| F-005 | Invoice | Tax & discount application | planned | Per-line or invoice-level |
| F-006 | Invoice | Invoice numbering (auto-increment, prefix) | planned | Customizable format |
| F-007 | Invoice | Invoice status lifecycle (draft/sent/viewed/paid/overdue) | planned | State machine |
| F-008 | Invoice | Duplicate / template from existing invoice | planned | Productivity |
| F-009 | Invoice | Invoice email delivery (SMTP via moon-server) | planned | Server-side send |
| F-010 | PDF | PDF export (single invoice) | planned | See ADR-002 for generation location |
| F-011 | PDF | PDF template selection (Pro) | planned | Multiple templates, Pro tier |
| F-012 | Payment | Mark payment (full / partial) | planned | Updates invoice status |
| F-013 | Payment | Payment history per invoice | planned | Audit trail |
| F-014 | Payment | Overdue detection (auto-scan) | planned | Scheduled check against payment terms |
| F-015 | Alert | Push notification for overdue invoices | planned | Local + server push |
| F-016 | Alert | Upcoming due date reminder (3-day, 1-day) | planned | Configurable |
| F-017 | Revenue | Monthly revenue chart | planned | Bar/line chart |
| F-018 | Revenue | Per-client revenue breakdown | planned | Pie/bar chart |
| F-019 | Revenue | Year-over-year comparison | planned | Pro tier |
| F-020 | Tax | Tax rate suggestion by locale | planned | `moon-i18n-kmp` tax module |
| F-021 | Tax | Quarterly tax estimation | planned | Based on accumulated revenue |
| F-022 | Tax | Tax summary export (CSV) | planned | For accountant handoff |
| F-023 | Time | Time tracking (start/stop timer) | planned | FocusOn core reuse |
| F-024 | Time | Manual time entry | planned | Fallback for non-timer users |
| F-025 | Time | Time-to-invoice conversion | planned | Auto-populate line items from logged hours |
| F-026 | Currency | Multi-currency display/conversion | planned | `moon-i18n-kmp` FormatCurrency |
| F-027 | Currency | Exchange rate fetch (daily) | planned | Server-side cron |
| F-028 | Auth | Login / logout / profile | planned | `moon-auth-kmp` |
| F-029 | Paywall | Pro subscription / limit enforcement | planned | `moon-billing-kmp` |
| F-030 | Settings | Invoice default settings (logo, company info, footer) | planned | Persisted locally + sync |
| F-031 | Export | Full data JSON backup/restore | planned | Data portability |
| F-032 | Feedback | In-app feedback submission | planned | Standard moon pattern |

**Total: 32 features, all planned.**

## Data Model (Planned)

```kotlin
// Expected location: freelancepay/composeApp/src/commonMain/kotlin/.../domain/model/

data class Client(
    val id: String,
    val name: String,
    val email: String,
    val company: String?,
    val address: String?,
    val defaultCurrency: String, // ISO 4217
    val paymentTermDays: Int,    // net-30 default
    val createdAt: Long
)

data class Invoice(
    val id: String,
    val invoiceNumber: String,   // e.g. "INV-2026-0042"
    val clientId: String,
    val status: InvoiceStatus,
    val lineItems: List<LineItem>,
    val taxRate: Double?,
    val discount: Double?,
    val currency: String,
    val issuedAt: Long,
    val dueAt: Long,
    val paidAt: Long?
)

enum class InvoiceStatus { DRAFT, SENT, VIEWED, PAID, PARTIALLY_PAID, OVERDUE, CANCELLED }

data class LineItem(
    val description: String,
    val quantity: Double,
    val unitPrice: Double
)

data class Payment(
    val id: String,
    val invoiceId: String,
    val amount: Double,
    val paidAt: Long,
    val method: String?
)

data class TimeEntry(
    val id: String,
    val projectId: String,
    val startAt: Long,
    val endAt: Long?,
    val durationMinutes: Int,
    val note: String?
)
```

## Module Structure (Planned)

- `freelancepay/libs/freelancepay-domain` — Domain models + UseCases
- `freelancepay/libs/freelancepay-data` — Repository implementations, local DB
- `freelancepay/composeApp` — UI (Compose Multiplatform, MVI ViewModels)
