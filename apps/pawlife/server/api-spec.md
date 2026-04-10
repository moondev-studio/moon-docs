# PawLife — Server API Spec (Design)

- Status: design-spec (planned, server not yet built)
- Date: 2026-04-10
- Base URL: `https://moon-server-production.up.railway.app/api/pawlife/v1`
- Auth: Firebase ID Token (Bearer)

## Architecture Notes
- Backend: moon-server (Spring Boot, Railway) — shared with all MoonDeveloper apps
- Data: Firestore (primary), with server-side validation and aggregation
- Pattern: Client-first with Firestore direct access for CRUD; server endpoints for computed data, team operations, and notifications
- 4-Tier: Server endpoints serve L3/L4 needs (push notifications, scheduled jobs, team invites)

## Endpoints

### Pet

| Method | Path                        | Description                     | Status  |
|--------|-----------------------------|---------------------------------|---------|
| GET    | `/pets`                     | List pets for authenticated user | planned |
| GET    | `/pets/{petId}`             | Get pet detail                  | planned |
| POST   | `/pets`                     | Create pet                      | planned |
| PUT    | `/pets/{petId}`             | Update pet                      | planned |
| DELETE | `/pets/{petId}`             | Delete pet                      | planned |
| GET    | `/pets/breeds`              | Get breed database (dog/cat)    | planned |
| GET    | `/pets/breeds/search?q={query}` | Search breeds               | planned |

### Walk

| Method | Path                        | Description                     | Status  |
|--------|-----------------------------|---------------------------------|---------|
| GET    | `/walks`                    | List walks (filter by petId, date range) | planned |
| GET    | `/walks/{walkId}`           | Get walk detail with route      | planned |
| POST   | `/walks`                    | Save completed walk session     | planned |
| GET    | `/walks/stats`              | Walk statistics (weekly/monthly aggregation) | planned |
| GET    | `/walks/stats/{petId}`      | Per-pet walk statistics         | planned |

### Health

| Method | Path                        | Description                     | Status  |
|--------|-----------------------------|---------------------------------|---------|
| GET    | `/health/records`           | List health records (filter by petId, type) | planned |
| GET    | `/health/records/{recordId}` | Get health record detail       | planned |
| POST   | `/health/records`           | Create health record            | planned |
| PUT    | `/health/records/{recordId}` | Update health record           | planned |
| DELETE | `/health/records/{recordId}` | Delete health record           | planned |
| GET    | `/health/upcoming`          | Upcoming vaccinations/visits (D-day list) | planned |
| GET    | `/health/costs`             | Health cost summary (monthly)   | planned |

### Feeding

| Method | Path                        | Description                     | Status  |
|--------|-----------------------------|---------------------------------|---------|
| GET    | `/feedings`                 | List feedings (filter by petId, date) | planned |
| POST   | `/feedings`                 | Add feeding record              | planned |
| GET    | `/feedings/today/{petId}`   | Today's feedings for a pet      | planned |
| GET    | `/feedings/stats/{petId}`   | Feeding statistics              | planned |

### Team

| Method | Path                        | Description                     | Status  |
|--------|-----------------------------|---------------------------------|---------|
| GET    | `/teams/{teamId}`           | Get team detail                 | planned |
| POST   | `/teams`                    | Create team                     | planned |
| POST   | `/teams/join`               | Join team by invite code        | planned |
| DELETE | `/teams/{teamId}/leave`     | Leave team                      | planned |
| GET    | `/teams/{teamId}/members`   | List team members               | planned |
| DELETE | `/teams/{teamId}/members/{userId}` | Remove member (owner only) | planned |
| POST   | `/teams/{teamId}/invite`    | Generate/refresh invite code    | planned |
| GET    | `/teams/{teamId}/activity`  | Team activity feed              | planned |

### Notification

| Method | Path                        | Description                     | Status  |
|--------|-----------------------------|---------------------------------|---------|
| POST   | `/notifications/register`   | Register FCM/APNs device token  | planned |
| PUT    | `/notifications/preferences` | Update notification preferences | planned |
| GET    | `/notifications/preferences` | Get notification preferences    | planned |

### Subscription

| Method | Path                        | Description                     | Status  |
|--------|-----------------------------|---------------------------------|---------|
| GET    | `/subscription/status`      | Current subscription status     | planned |
| POST   | `/subscription/verify`      | Verify purchase receipt         | planned |

### Export

| Method | Path                        | Description                     | Status  |
|--------|-----------------------------|---------------------------------|---------|
| GET    | `/export/csv`               | Export all pet data as CSV      | planned |
| GET    | `/export/json`              | Export all data as JSON backup  | planned |
| POST   | `/import/json`              | Restore from JSON backup        | planned |

## Request/Response Examples (Planned)

### POST /pets
```json
// Request
{
  "name": "Momo",
  "type": "DOG",
  "breed": "Pomeranian",
  "gender": "FEMALE",
  "birthDate": 1672531200000,
  "isNeutered": true,
  "weightKg": 3.2,
  "teamId": "team_abc123"
}

// Response 201
{
  "id": "pet_xyz789",
  "name": "Momo",
  "type": "DOG",
  "breed": "Pomeranian",
  "gender": "FEMALE",
  "birthDate": 1672531200000,
  "isNeutered": true,
  "weightKg": 3.2,
  "photoUrl": "",
  "teamId": "team_abc123",
  "ownerId": "user_123",
  "createdAt": 1712700000000
}
```

### POST /walks
```json
// Request
{
  "petId": "pet_xyz789",
  "startTime": 1712700000000,
  "endTime": 1712701800000,
  "distanceMeters": 2340.5,
  "durationSeconds": 1800,
  "routePoints": [[37.5665, 126.9780], [37.5670, 126.9785]],
  "caloriesKcal": 95.2
}

// Response 201
{
  "id": "walk_abc456",
  "petId": "pet_xyz789",
  "userId": "user_123",
  "startTime": 1712700000000,
  "endTime": 1712701800000,
  "distanceMeters": 2340.5,
  "durationSeconds": 1800,
  "caloriesKcal": 95.2,
  "isCompleted": true
}
```

## Endpoint Count Summary
- Pet: 7 endpoints
- Walk: 5 endpoints
- Health: 7 endpoints
- Feeding: 4 endpoints
- Team: 8 endpoints
- Notification: 3 endpoints
- Subscription: 2 endpoints
- Export: 3 endpoints
- **Total: 39 endpoints**
