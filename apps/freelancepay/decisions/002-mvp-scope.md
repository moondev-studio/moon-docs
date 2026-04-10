# ADR-002: MVP Scope and PDF Generation Strategy

- Version: 0.1.0
- Date: 2026-04-10
- Status: Proposed
- Deciders: developer_1 (changson)

## Context

FreelancePay is entering the design phase. Two key decisions need to be made:
1. What constitutes the MVP feature set?
2. Where should PDF invoice generation happen — client-side or server-side?

## Decision 1: MVP Feature Scope

### Included in MVP

| Area | Features | Rationale |
|------|----------|-----------|
| Client Management | CRUD, default currency, payment terms | Core entity — everything depends on clients |
| Invoice Creation | Line items, tax, discount, numbering, status lifecycle | Primary value proposition |
| PDF Export | Single-template PDF generation | Must-have for sending invoices |
| Payment Tracking | Mark paid/partial, payment history | Freelancers need to know who paid |
| Overdue Alerts | Auto-detection, push notifications | Key differentiator — "nothing slips through" |
| Revenue Stats | Monthly chart, per-client breakdown | Quick insight without spreadsheets |
| Time Tracking | Timer + manual entry, time-to-invoice | Hourly freelancers need this |
| Auth | Login/logout via moon-auth-kmp | Required for data sync |

### Deferred to Post-MVP

| Feature | Reason |
|---------|--------|
| Multiple PDF templates (Pro) | Nice-to-have; single template suffices for launch |
| Year-over-year comparison | Requires 12+ months of data to be useful |
| Quarterly tax estimation | Complex per-locale rules; needs validation |
| Tax summary CSV export | Dependent on tax estimation |
| Full JSON backup/restore | Lower priority; can use server backup |
| In-app feedback | Standard pattern, easy to add later |

## Decision 2: PDF Generation — Client vs Server

### Options Considered

#### Option A: Server-Side (moon-server, JasperReports / iText)

**Pros:**
- Consistent rendering across all platforms (Android, iOS, Web)
- Full font/layout control without platform constraints
- Template management centralized — update once, all clients get it
- Can attach PDF directly to email in single server call
- No large PDF library bundled into mobile app (~2-5 MB savings)

**Cons:**
- Requires network connectivity to generate PDF
- Server load scales with invoice volume
- Adds latency (~1-3 sec for generation)

#### Option B: Client-Side (KMP expect/actual per platform)

**Pros:**
- Works offline
- No server dependency for core feature
- Faster for single-invoice generation

**Cons:**
- Must implement PDF generation 3 times (Android, iOS, Web)
- Platform-specific rendering differences (fonts, layout)
- Larger app binary size
- Cannot easily update templates without app release

#### Option C: Hybrid (client generates, server as fallback)

**Pros:**
- Offline capability + consistency via server fallback

**Cons:**
- Most complex to implement and maintain
- Two rendering paths to test and keep in sync

### Decision

**Option A: Server-Side PDF generation** via moon-server.

### Rationale

1. **Consistency is critical for invoices** — clients receive these documents; visual quality matters
2. **Template iteration speed** — can update PDF templates without app release
3. **Email integration** — server already handles SMTP; generating PDF server-side enables single-call "generate + email" flow
4. **FreelancePay is web-primary** — web users always have connectivity
5. **App binary size** — avoiding PDF libraries keeps mobile app lean
6. **Offline concern is minimal** — invoices are typically created in connected environments; draft can be saved locally and PDF generated when online

### Consequences

- Invoice PDF generation requires network connectivity
- Server must handle concurrent PDF generation load (consider queue for burst traffic)
- Need to implement PDF template engine on moon-server (JasperReports or iText)
- Client stores invoice data locally; PDF is generated on-demand from server
- Offline draft workflow: create invoice offline, sync and generate PDF when online

## Impact

- Scope: Medium — affects architecture of invoice module and server workload
- Timeline: PDF template implementation is on the critical path for MVP
- Dependencies: moon-server must add `/api/freelancepay/v1/invoices/{id}/pdf` endpoint

## References

- ADR-001: Initial Design (4-Tier KMP architecture)
- FreelancePay overview.md — revenue model and tech stack
- FocusOn timer reuse strategy
