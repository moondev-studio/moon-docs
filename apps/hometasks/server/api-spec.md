# HomeTasks — Server API Spec

- Version: v1.0.0
- Date: 2026-04-10
- Base URL: `https://moon-server-production.up.railway.app/api/hometasks/v1`
- Status: All endpoints are **planned** (design-first)
- Auth: Firebase ID Token (Bearer)

## Endpoints

### Tasks

| Method | Path                          | Description                        | Request Body                     | Response           | Status  |
|--------|-------------------------------|------------------------------------|----------------------------------|--------------------|---------|
| GET    | `/tasks`                      | List tasks for current team        | --                               | `Task[]`           | planned |
| POST   | `/tasks`                      | Create a new task                  | `CreateTaskRequest`              | `Task`             | planned |
| GET    | `/tasks/{taskId}`             | Get task detail                    | --                               | `Task`             | planned |
| PUT    | `/tasks/{taskId}`             | Update task                        | `UpdateTaskRequest`              | `Task`             | planned |
| DELETE | `/tasks/{taskId}`             | Delete task                        | --                               | `204 No Content`   | planned |
| POST   | `/tasks/{taskId}/complete`    | Mark task as completed             | `CompleteTaskRequest`            | `TaskCompletion`   | planned |
| DELETE | `/tasks/{taskId}/complete`    | Undo task completion               | --                               | `204 No Content`   | planned |
| POST   | `/tasks/{taskId}/photo`       | Upload completion photo            | `multipart/form-data`            | `PhotoUploadResponse` | planned |

### Team

| Method | Path                          | Description                        | Request Body                     | Response           | Status  |
|--------|-------------------------------|------------------------------------|----------------------------------|--------------------|---------|
| POST   | `/teams`                      | Create a new team                  | `CreateTeamRequest`              | `Team`             | planned |
| GET    | `/teams/{teamId}`             | Get team info                      | --                               | `Team`             | planned |
| GET    | `/teams/{teamId}/members`     | List team members                  | --                               | `TeamMember[]`     | planned |
| POST   | `/teams/{teamId}/invite`      | Generate invite link/code          | `CreateInviteRequest`            | `InviteLink`       | planned |
| POST   | `/teams/join`                 | Join team via invite code          | `JoinTeamRequest`                | `TeamMember`       | planned |
| DELETE | `/teams/{teamId}/members/{memberId}` | Remove member from team     | --                               | `204 No Content`   | planned |
| PUT    | `/teams/{teamId}/members/{memberId}` | Update member role           | `UpdateRoleRequest`              | `TeamMember`       | planned |

### Rotation

| Method | Path                          | Description                        | Request Body                     | Response           | Status  |
|--------|-------------------------------|------------------------------------|----------------------------------|--------------------|---------|
| GET    | `/teams/{teamId}/rotations`   | Get rotation schedules             | --                               | `RotationSchedule[]` | planned |
| POST   | `/teams/{teamId}/rotations`   | Create rotation schedule           | `CreateRotationRequest`          | `RotationSchedule` | planned |
| PUT    | `/teams/{teamId}/rotations/{rotationId}` | Update rotation config  | `UpdateRotationRequest`          | `RotationSchedule` | planned |
| DELETE | `/teams/{teamId}/rotations/{rotationId}` | Delete rotation          | --                               | `204 No Content`   | planned |
| POST   | `/teams/{teamId}/rotations/advance` | Manually advance rotation   | --                               | `RotationSchedule[]` | planned |
| POST   | `/teams/{teamId}/swaps`       | Request task swap                  | `SwapRequest`                    | `SwapRequest`      | planned |
| PUT    | `/teams/{teamId}/swaps/{swapId}` | Approve/reject swap             | `SwapDecision`                   | `SwapRequest`      | planned |

### Stats

| Method | Path                          | Description                        | Request Body                     | Response           | Status  |
|--------|-------------------------------|------------------------------------|----------------------------------|--------------------|---------|
| GET    | `/teams/{teamId}/stats`       | Team completion stats overview     | --                               | `TeamStats`        | planned |
| GET    | `/teams/{teamId}/stats/workload` | Workload distribution           | --                               | `WorkloadStats`    | planned |
| GET    | `/teams/{teamId}/stats/weekly` | Weekly summary report             | --                               | `WeeklyReport`     | planned |
| GET    | `/teams/{teamId}/stats/monthly` | Monthly trend data               | --                               | `MonthlyTrend`     | planned |

### Rewards

| Method | Path                          | Description                        | Request Body                     | Response           | Status  |
|--------|-------------------------------|------------------------------------|----------------------------------|--------------------|---------|
| GET    | `/teams/{teamId}/rewards`     | Team reward summary                | --                               | `RewardSummary`    | planned |
| GET    | `/teams/{teamId}/rewards/{memberId}` | Member reward detail         | --                               | `RewardPoints`     | planned |
| GET    | `/teams/{teamId}/rewards/leaderboard` | Points leaderboard          | --                               | `LeaderboardEntry[]` | planned |
| GET    | `/teams/{teamId}/rewards/history` | Reward history log              | --                               | `RewardEvent[]`    | planned |
| GET    | `/teams/{teamId}/badges`      | Awarded badges for team            | --                               | `Badge[]`          | planned |

### Auth and Profile

| Method | Path                          | Description                        | Request Body                     | Response           | Status  |
|--------|-------------------------------|------------------------------------|----------------------------------|--------------------|---------|
| GET    | `/profile`                    | Get current user profile           | --                               | `UserProfile`      | planned |
| PUT    | `/profile`                    | Update profile                     | `UpdateProfileRequest`           | `UserProfile`      | planned |

### Notification

| Method | Path                          | Description                        | Request Body                     | Response           | Status  |
|--------|-------------------------------|------------------------------------|----------------------------------|--------------------|---------|
| PUT    | `/profile/notifications`      | Update notification preferences    | `NotificationPrefRequest`        | `NotificationPref` | planned |

### Feedback

| Method | Path                          | Description                        | Request Body                     | Response           | Status  |
|--------|-------------------------------|------------------------------------|----------------------------------|--------------------|---------|
| POST   | `/feedback`                   | Submit user feedback               | `FeedbackRequest`                | `201 Created`      | planned |

## Endpoint Count: 33

## Request/Response Models (planned)

```json
// CreateTaskRequest
{
  "title": "string",
  "category": "KITCHEN | BATHROOM | LIVING_ROOM | ...",
  "estimatedMinutes": 30,
  "priority": "HIGH | MEDIUM | LOW",
  "assigneeId": "string?",
  "recurrence": {
    "type": "DAILY | WEEKLY | BIWEEKLY | MONTHLY | CUSTOM",
    "interval": 1,
    "daysOfWeek": [1, 3, 5]
  }
}

// CompleteTaskRequest
{
  "completedAt": 1712700000000,
  "note": "string?"
}

// CreateInviteRequest
{
  "expiresInHours": 48
}

// JoinTeamRequest
{
  "inviteCode": "string"
}

// SwapRequest
{
  "fromTaskId": "string",
  "toMemberId": "string",
  "reason": "string?"
}

// SwapDecision
{
  "approved": true,
  "reason": "string?"
}
```

## Error Response Format

```json
{
  "error": {
    "code": "TASK_NOT_FOUND",
    "message": "Task with id xyz not found",
    "status": 404
  }
}
```
