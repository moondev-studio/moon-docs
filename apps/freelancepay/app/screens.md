# FreelancePay — Screen Spec

- Version: 0.1.0
- Date: 2026-04-10
- Status: All screens **planned**
- Ref: `FreelancePay/composeApp/.../navigation/Screen.kt` (current scaffold)

## Screen Inventory

| ID | Route | Screen Name | Description | Status |
|----|-------|-------------|-------------|--------|
| S-001 | `dashboard` | DashboardScreen | Revenue summary, overdue count, recent invoices, quick actions | planned |
| S-002 | `clients` | ClientListScreen | Client list with search/filter | planned |
| S-003 | `clients/{id}` | ClientDetailScreen | Client profile, invoice history, revenue for client | planned |
| S-004 | `clients/new` | ClientEditScreen | Add/edit client form | planned |
| S-005 | `projects` | ProjectListScreen | Project list (existing scaffold) | planned |
| S-006 | `projects/{id}` | ProjectDetailScreen | Project detail with time entries | planned |
| S-007 | `invoices` | InvoiceListScreen | All invoices with status filter (draft/sent/paid/overdue) | planned |
| S-008 | `invoices/new?clientId={id}` | InvoiceCreateScreen | Invoice builder — line items, tax, discount, due date | planned |
| S-009 | `invoices/{id}` | InvoiceDetailScreen | Invoice preview, status actions (send/mark paid), PDF export | planned |
| S-010 | `invoices/{id}/edit` | InvoiceEditScreen | Edit draft invoice | planned |
| S-011 | `invoices/{id}/pdf` | InvoicePdfPreviewScreen | Full-screen PDF preview before export/send | planned |
| S-012 | `timer/{projectId}` | TimerScreen | Time tracker (FocusOn core reuse) (existing scaffold) | planned |
| S-013 | `time-entries` | TimeEntryListScreen | Manual time entry list, bulk convert to invoice | planned |
| S-014 | `revenue` | RevenueStatsScreen | Monthly chart, client breakdown, YoY comparison | planned |
| S-015 | `revenue/tax` | TaxEstimationScreen | Quarterly tax estimate, locale-based rates | planned |
| S-016 | `alerts` | AlertListScreen | Overdue and upcoming-due notification history | planned |
| S-017 | `settings` | SettingsScreen | App settings (existing scaffold) | planned |
| S-018 | `settings/invoice-defaults` | InvoiceDefaultsScreen | Logo, company info, footer, numbering format | planned |
| S-019 | `settings/currency` | CurrencySettingsScreen | Default currency, exchange rate display | planned |
| S-020 | `paywall` | PaywallScreen | Pro subscription purchase | planned |
| S-021 | `export` | DataExportScreen | JSON backup / CSV tax export | planned |
| S-022 | `feedback` | FeedbackScreen | In-app feedback form | planned |

**Total: 22 screens, all planned.**

## Navigation Notes

- Current scaffold defines 5 routes: `dashboard`, `projects`, `timer/{projectId}`, `invoice/{projectId}`, `settings`
- MVP will expand to the 22 screens above
- Bottom navigation tabs: Dashboard, Clients, Invoices, Revenue, Settings
- Deep-link support planned for invoice sharing

## Screen-Feature Mapping

| Screen | Primary Features |
|--------|-----------------|
| DashboardScreen | F-017, F-014, F-015 |
| ClientListScreen / Detail / Edit | F-001, F-002, F-003 |
| InvoiceCreateScreen / Edit | F-004, F-005, F-006, F-008 |
| InvoiceDetailScreen | F-007, F-009, F-010, F-012 |
| InvoicePdfPreviewScreen | F-010, F-011 |
| TimerScreen | F-023 |
| TimeEntryListScreen | F-024, F-025 |
| RevenueStatsScreen | F-017, F-018, F-019 |
| TaxEstimationScreen | F-020, F-021, F-022 |
| AlertListScreen | F-015, F-016 |
| PaywallScreen | F-029 |
