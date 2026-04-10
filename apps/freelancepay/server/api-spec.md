# FreelancePay — Server API Spec

- Version: 0.1.0
- Date: 2026-04-10
- Status: All endpoints **planned**
- Base URL: `https://moon-server-production.up.railway.app/api/freelancepay/v1`

## Authentication

All endpoints require `Authorization: Bearer <firebase-id-token>` header.
User identity is extracted from the token; all data is scoped to the authenticated user.

## Endpoints

### Clients

| Method | Path | Description | Status |
|--------|------|-------------|--------|
| GET | `/clients` | List all clients (paginated) | planned |
| GET | `/clients/{id}` | Get client detail | planned |
| POST | `/clients` | Create client | planned |
| PUT | `/clients/{id}` | Update client | planned |
| DELETE | `/clients/{id}` | Delete client (soft delete) | planned |

**Request/Response (POST /clients):**
```json
// Request
{
  "name": "Acme Corp",
  "email": "billing@acme.com",
  "company": "Acme Corp",
  "address": "123 Main St, SF, CA",
  "defaultCurrency": "USD",
  "paymentTermDays": 30
}

// Response: 201 Created
{
  "id": "cli_abc123",
  "name": "Acme Corp",
  "email": "billing@acme.com",
  "company": "Acme Corp",
  "address": "123 Main St, SF, CA",
  "defaultCurrency": "USD",
  "paymentTermDays": 30,
  "createdAt": "2026-04-10T09:00:00Z"
}
```

### Invoices

| Method | Path | Description | Status |
|--------|------|-------------|--------|
| GET | `/invoices` | List invoices (filter by status, clientId, date range) | planned |
| GET | `/invoices/{id}` | Get invoice detail | planned |
| POST | `/invoices` | Create invoice | planned |
| PUT | `/invoices/{id}` | Update invoice (draft only) | planned |
| DELETE | `/invoices/{id}` | Delete invoice (draft only) | planned |
| POST | `/invoices/{id}/send` | Send invoice via email | planned |
| POST | `/invoices/{id}/mark-paid` | Mark invoice as paid | planned |
| POST | `/invoices/{id}/mark-partial` | Record partial payment | planned |

**Request (POST /invoices):**
```json
{
  "clientId": "cli_abc123",
  "invoiceNumber": "INV-2026-0042",
  "lineItems": [
    { "description": "Web Development", "quantity": 40, "unitPrice": 75.00 },
    { "description": "Design Review", "quantity": 5, "unitPrice": 100.00 }
  ],
  "taxRate": 0.10,
  "discount": 50.00,
  "currency": "USD",
  "dueAt": "2026-05-10T00:00:00Z",
  "notes": "Thank you for your business!"
}
```

**Request (POST /invoices/{id}/mark-partial):**
```json
{
  "amount": 1500.00,
  "paidAt": "2026-04-15T14:30:00Z",
  "method": "bank_transfer"
}
```

### PDF Generation

| Method | Path | Description | Status |
|--------|------|-------------|--------|
| GET | `/invoices/{id}/pdf` | Generate and return invoice PDF | planned |
| GET | `/invoices/{id}/pdf?template={name}` | Generate PDF with specific template (Pro) | planned |

> PDF generation strategy (client-side vs server-side) is discussed in ADR-002.
> Current plan leans toward **server-side** (JasperReports/iText on moon-server).

### Payments

| Method | Path | Description | Status |
|--------|------|-------------|--------|
| GET | `/invoices/{id}/payments` | List payments for an invoice | planned |
| POST | `/invoices/{id}/payments` | Record a payment | planned |
| DELETE | `/invoices/{id}/payments/{paymentId}` | Delete a payment record | planned |

### Revenue & Statistics

| Method | Path | Description | Status |
|--------|------|-------------|--------|
| GET | `/stats/revenue?period=monthly&year=2026` | Monthly revenue data | planned |
| GET | `/stats/revenue/by-client?year=2026` | Per-client revenue breakdown | planned |
| GET | `/stats/revenue/yoy` | Year-over-year comparison | planned |
| GET | `/stats/tax-estimate?quarter=Q2&year=2026` | Quarterly tax estimation | planned |

### Time Entries

| Method | Path | Description | Status |
|--------|------|-------------|--------|
| GET | `/time-entries?projectId={id}` | List time entries | planned |
| POST | `/time-entries` | Create time entry | planned |
| PUT | `/time-entries/{id}` | Update time entry | planned |
| DELETE | `/time-entries/{id}` | Delete time entry | planned |
| POST | `/time-entries/convert-to-invoice` | Bulk convert time entries to invoice line items | planned |

**Request (POST /time-entries/convert-to-invoice):**
```json
{
  "timeEntryIds": ["te_001", "te_002", "te_003"],
  "clientId": "cli_abc123",
  "hourlyRate": 75.00
}
```

### Alerts & Notifications

| Method | Path | Description | Status |
|--------|------|-------------|--------|
| GET | `/alerts/overdue` | Get current overdue invoices | planned |
| POST | `/alerts/scan` | Trigger overdue scan (also runs on server cron) | planned |
| PUT | `/alerts/settings` | Update notification preferences | planned |

### Export

| Method | Path | Description | Status |
|--------|------|-------------|--------|
| GET | `/export/data` | Full JSON data export | planned |
| POST | `/export/import` | Import JSON backup | planned |
| GET | `/export/tax-summary?year=2026` | Tax summary CSV export | planned |

### Exchange Rates

| Method | Path | Description | Status |
|--------|------|-------------|--------|
| GET | `/exchange-rates?base=USD&targets=KRW,JPY,EUR` | Get latest exchange rates | planned |

## Endpoint Summary

**Total: 31 endpoints, all planned.**

| Area | Count |
|------|-------|
| Clients | 5 |
| Invoices | 8 |
| PDF | 2 |
| Payments | 3 |
| Revenue/Stats | 4 |
| Time Entries | 5 |
| Alerts | 3 |
| Export | 3 |
| Exchange Rates | 1 |

## Error Response Format

```json
{
  "error": {
    "code": "INVOICE_NOT_FOUND",
    "message": "Invoice with id inv_xyz not found",
    "status": 404
  }
}
```

## Pagination

List endpoints support cursor-based pagination:
```
GET /invoices?cursor=eyJ...&limit=20
```

Response includes:
```json
{
  "data": [...],
  "nextCursor": "eyJ...",
  "hasMore": true
}
```
