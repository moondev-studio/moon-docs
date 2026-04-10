# CoupleSync — Server API Spec

- Version: v1.0.0
- Date: 2026-04-10
- Base path: `/api/couplesync/v1`
- Auth: Bearer Token (Spring Security)
- Source: `moon-server/src/main/kotlin/com/moondeveloper/server/couplesync/controller/CoupleSyncController.kt`

## REST Endpoints

| Method   | Path                                        | Description                              | Auth |
|----------|---------------------------------------------|------------------------------------------|------|
| GET      | `/health`                                   | Health check                             | No   |
| GET      | `/couples/me`                               | Get current user's couple pair           | Yes  |
| DELETE   | `/couples/me`                               | Dissolve couple (set DISSOLVED)          | Yes  |
| POST     | `/invitations`                              | Create pairing invitation (24h TTL)      | Yes  |
| POST     | `/invitations/{code}/accept`                | Accept invitation and create couple pair | Yes  |
| GET      | `/invitations/{code}/preview`               | Preview invitation validity              | No   |
| GET      | `/couples/{pairId}/ledger/entries`          | List all ledger entries                  | Yes  |
| POST     | `/couples/{pairId}/ledger/entries`          | Add ledger entry                         | Yes  |
| PATCH    | `/couples/{pairId}/ledger/entries/{id}`     | Update ledger entry                      | Yes  |
| DELETE   | `/couples/{pairId}/ledger/entries/{id}`     | Delete ledger entry                      | Yes  |
| GET      | `/couples/{pairId}/ledger/stats/monthly`    | Monthly income/expense/balance stats     | Yes  |
| GET      | `/couples/{pairId}/ledger/stats/split`      | Split contribution stats per user        | Yes  |
| GET      | `/couples/{pairId}/settlements`             | List settlements                         | Yes  |
| POST     | `/couples/{pairId}/settlements`             | Create settlement                        | Yes  |
| POST     | `/couples/{pairId}/settlements/{id}/settle` | Mark settlement as settled               | Yes  |
| GET      | `/couples/{pairId}/goals`                   | List active goals                        | Yes  |
| POST     | `/couples/{pairId}/goals`                   | Create saving goal                       | Yes  |
| PATCH    | `/couples/{pairId}/goals/{id}/amount`       | Update goal current amount               | Yes  |
| POST     | `/couples/{pairId}/goals/{id}/complete`     | Mark goal as completed                   | Yes  |
| GET      | `/couples/{pairId}/events`                  | List all calendar events                 | Yes  |
| GET      | `/couples/{pairId}/events/upcoming`         | List upcoming events                     | Yes  |
| POST     | `/couples/{pairId}/events`                  | Create calendar event                    | Yes  |
| PATCH    | `/couples/{pairId}/events/{id}`             | Update calendar event                    | Yes  |
| DELETE   | `/couples/{pairId}/events/{id}`             | Delete calendar event                    | Yes  |
| GET      | `/couples/{pairId}/memories`                | List memories                            | Yes  |
| POST     | `/couples/{pairId}/memories`                | Add memory                               | Yes  |
| DELETE   | `/couples/{pairId}/memories/{id}`           | Delete memory                            | Yes  |

Total: 27 endpoints

## WebSocket

| Destination                                           | Direction | Description                    |
|-------------------------------------------------------|-----------|--------------------------------|
| `/app/couplesync/couples/{pairId}/presence`            | Client -> Server | Send online/offline status |
| `/topic/couplesync/couples/{pairId}`                   | Server -> Client | Broadcast all CRUD events  |

### WebSocket Event Types

| Event Type          | Trigger                  |
|---------------------|--------------------------|
| LEDGER_ADDED        | Ledger entry created     |
| LEDGER_UPDATED      | Ledger entry updated     |
| LEDGER_DELETED      | Ledger entry deleted     |
| SETTLEMENT_CREATED  | Settlement created       |
| SETTLEMENT_SETTLED  | Settlement marked settled|
| GOAL_UPDATED        | Goal amount updated      |
| GOAL_COMPLETED      | Goal completed           |
| EVENT_ADDED         | Calendar event created   |
| EVENT_UPDATED       | Calendar event updated   |
| EVENT_DELETED       | Calendar event deleted   |
| MEMORY_ADDED        | Memory created           |
| PARTNER_ONLINE      | Partner came online      |
| PARTNER_OFFLINE     | Partner went offline     |

## Request/Response DTOs

### LedgerEntryDto
```json
{
  "title": "string",
  "amount": 10000,
  "type": "INCOME | EXPENSE",
  "category": "FOOD | TRANSPORT | CAFE | ...",
  "scope": "SHARED | PERSONAL (optional, default: SHARED)",
  "memo": "string (optional)",
  "date": "2026-04-10 (optional, default: today)"
}
```

### SettlementDto
```json
{
  "title": "string",
  "user1Amount": 50000,
  "user2Amount": 30000
}
```

### GoalDto
```json
{
  "title": "string",
  "description": "string (optional)",
  "targetAmount": 1000000,
  "emoji": "string (optional, default: target emoji)",
  "targetDate": "2026-12-31 (optional)"
}
```

### EventDto
```json
{
  "title": "string",
  "description": "string (optional)",
  "date": "2026-04-10",
  "emoji": "string (optional, default: calendar emoji)",
  "isAnniversary": false,
  "isRecurringYearly": false
}
```

### MemoryDto
```json
{
  "title": "string",
  "description": "string (optional)",
  "date": "2026-04-10 (optional, default: today)",
  "photoUrl": "string (optional)",
  "emoji": "string (optional, default: heart emoji)"
}
```
