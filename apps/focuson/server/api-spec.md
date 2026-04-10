# FocusOn — Server API Spec

- Version: v1.0.0
- Date: 2026-04-10
- Base path: `/api/focuson/v1`
- Auth: Bearer token (Spring Security)
- Source: `moon-server/src/main/kotlin/com/moondeveloper/server/focuson/`

## REST Endpoints

| Method   | Path                          | Description                       | Auth | Request Body                  | Response                  |
|----------|-------------------------------|-----------------------------------|------|-------------------------------|---------------------------|
| GET      | `/health`                     | FocusOn health check              | No   | --                            | `ApiResponse<Map>`        |
| POST     | `/sessions/start`             | Start a new focus session         | Yes  | `StartSessionRequest`         | `FocusSession`            |
| POST     | `/sessions/{id}/complete`     | Complete a focus session          | Yes  | `CompleteSessionRequest`      | `FocusSession`            |
| GET      | `/sessions`                   | Get session history (by user)     | Yes  | --                            | `List<FocusSession>`      |
| GET      | `/sessions/stats/daily`       | Get daily focus time stats        | Yes  | --                            | `{userId, date, totalSeconds, totalMinutes}` |
| GET      | `/sessions/stats/weekly`      | Get weekly focus time stats       | Yes  | --                            | `{userId, totalSeconds, totalMinutes}` |
| GET      | `/rooms`                      | List public study rooms           | Yes  | --                            | `List<StudyRoom>`         |
| POST     | `/rooms`                      | Create a new study room           | Yes  | `CreateRoomRequest`           | `StudyRoom`               |
| GET      | `/rooms/{id}`                 | Get study room detail             | Yes  | --                            | `StudyRoom`               |
| POST     | `/rooms/{id}/join`            | Join a study room                 | Yes  | `JoinRoomRequest`             | `JoinResult`              |
| DELETE   | `/rooms/{id}/leave`           | Leave a study room                | Yes  | --                            | `204 No Content`          |
| GET      | `/rooms/{id}/status`          | Get study room live status        | Yes  | --                            | `RoomStatus`              |

## WebSocket Endpoints

| Destination                                | Description                                | Payload         |
|--------------------------------------------|--------------------------------------------|-----------------|
| `/focuson/rooms/{roomId}/action` (STOMP)   | Send real-time room action (status change, timer sync) | `RoomAction`    |

## Request/Response DTOs

### StartSessionRequest
```json
{
  "subjectId": "string?",
  "type": "POMODORO | STOPWATCH | CUSTOM",
  "soundscape": "string?",
  "roomId": "string?"
}
```

### CompleteSessionRequest
```json
{
  "durationSeconds": 1500
}
```

### CreateRoomRequest
```json
{
  "name": "string",
  "description": "string?",
  "maxMembers": 10,
  "isPublic": true,
  "weeklyGoalMinutes": 0
}
```

### JoinRoomRequest
```json
{
  "displayName": "string"
}
```

### RoomAction (WebSocket)
```json
{
  "type": "string",
  "displayName": "string?",
  "status": "FOCUSING | BREAK | IDLE",
  "timerRemaining": 1200
}
```

### RoomEvent (WebSocket broadcast)
```json
{
  "type": "MEMBER_JOINED | MEMBER_LEFT | STATUS_CHANGED | TIMER_START | TIMER_SYNC | ROOM_STATUS",
  "roomId": "string",
  "userId": "string?",
  "displayName": "string?",
  "status": "FOCUSING | BREAK | IDLE",
  "members": [{"userId": "...", "displayName": "...", "status": "..."}],
  "timerRemaining": 1200,
  "timestamp": 1712700000000
}
```

## Domain Models (Server)

### FocusSession
```kotlin
@Entity("focuson_sessions")
data class FocusSession(
    id: String,
    userId: String,
    subjectId: String?,
    startTime: Instant,
    endTime: Instant?,
    durationSeconds: Long,
    type: SessionType,       // POMODORO, STOPWATCH, CUSTOM
    soundscape: String?,
    completed: Boolean,
    roomId: String?,
    createdAt: Instant
)
```

### StudyRoom
```kotlin
@Entity("focuson_study_rooms")
data class StudyRoom(
    id: String,
    ownerId: String,
    name: String,
    description: String,
    maxMembers: Int,
    isPublic: Boolean,
    weeklyGoalMinutes: Int,
    status: RoomStatus,      // ACTIVE, CLOSED
    createdAt: Instant,
    updatedAt: Instant
)
```

### RoomMember
```kotlin
@Entity("focuson_room_members")
data class RoomMember(
    id: String,
    roomId: String,
    userId: String,
    displayName: String,
    status: MemberStatus,    // FOCUSING, BREAK, IDLE
    joinedAt: Instant
)
```
