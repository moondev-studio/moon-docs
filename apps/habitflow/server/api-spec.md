# HabitFlow — Server API Spec

- Version: v1.0.0
- Date: 2026-04-10
- Source: `moon-server/src/main/kotlin/.../habitflow/controller/HabitFlowController.kt`, `HabitflowHealthController.kt`
- Base path: `/api/habitflow/v1`
- Auth: Bearer token (Spring Security), all endpoints except `/health` require authentication

## Endpoints

### Health

| Method | Path      | Description              | Auth |
|--------|-----------|--------------------------|------|
| GET    | `/health` | Service health check     | No   |

### Habits

| Method | Path                         | Description                          | Request Body          | Auth |
|--------|------------------------------|--------------------------------------|-----------------------|------|
| GET    | `/habits`                    | List active habits for current user  | --                    | Yes  |
| POST   | `/habits`                    | Create a new habit                   | `CreateHabitDto`      | Yes  |
| GET    | `/habits/{id}`               | Get single habit by ID               | --                    | Yes  |
| PATCH  | `/habits/{id}/info`          | Update habit info (title, icon, etc) | `UpdateHabitInfoDto`  | Yes  |
| PATCH  | `/habits/{id}/schedule`      | Update habit schedule/frequency      | `UpdateScheduleDto`   | Yes  |
| POST   | `/habits/{id}/archive`       | Archive a habit (soft delete)        | --                    | Yes  |
| DELETE | `/habits/{id}`               | Permanently delete a habit           | --                    | Yes  |

### Check-ins

| Method | Path                           | Description                          | Request Body      | Auth |
|--------|--------------------------------|--------------------------------------|--------------------|------|
| POST   | `/habits/{id}/checkins`        | Record a check-in for a habit        | `CheckinRequest`   | Yes  |
| DELETE | `/habits/{id}/checkins/today`  | Cancel today's check-in              | --                 | Yes  |
| GET    | `/habits/{id}/checkins`        | List all check-ins for a habit       | --                 | Yes  |

### Streaks & Statistics

| Method | Path                     | Description                              | Query Params        | Auth |
|--------|--------------------------|------------------------------------------|---------------------|------|
| GET    | `/habits/{id}/streak`    | Get streak info for a habit              | --                  | Yes  |
| GET    | `/stats/today`           | Today's completion stats                 | --                  | Yes  |
| GET    | `/stats/weekly`          | Weekly completion stats (7-day window)   | --                  | Yes  |
| GET    | `/stats/heatmap`         | Yearly heatmap (daily check-in counts)   | `year` (default: current) | Yes  |

### Challenges

| Method | Path                              | Description                       | Request Body          | Auth    |
|--------|-----------------------------------|-----------------------------------|-----------------------|---------|
| GET    | `/challenges`                     | List public active challenges     | --                    | No      |
| GET    | `/challenges/{id}`                | Get single challenge detail       | --                    | No      |
| POST   | `/challenges`                     | Create a new challenge            | `CreateChallengeDto`  | Yes     |
| POST   | `/challenges/{id}/join`           | Join a challenge                  | --                    | Yes     |
| DELETE | `/challenges/{id}/leave`          | Leave a challenge                 | --                    | Yes     |
| GET    | `/challenges/{id}/leaderboard`    | Get challenge leaderboard         | --                    | No      |

## Request/Response DTOs

### CreateHabitDto

```kotlin
data class CreateHabitDto(
    val title: String,
    val description: String? = null,
    val icon: String? = null,        // default: "star"
    val color: String? = null,       // default: "#6B4EFF"
    val category: String? = null,    // default: "general"
    val frequency: HabitFrequency? = null,  // DAILY | WEEKLY | CUSTOM
    val weekdays: List<Int>? = null, // 1=Mon..7=Sun
    val reminderTime: String? = null,
    val goalStreak: Int? = null,     // default: 21
    val startDate: LocalDate? = null // default: today
)
```

### UpdateHabitInfoDto

```kotlin
data class UpdateHabitInfoDto(
    val title: String? = null,
    val description: String? = null,
    val icon: String? = null,
    val color: String? = null,
    val category: String? = null
)
```

### UpdateScheduleDto

```kotlin
data class UpdateScheduleDto(
    val frequency: HabitFrequency? = null,
    val weekdays: List<Int>? = null,
    val reminderTime: String? = null
)
```

### CheckinRequest

```kotlin
data class CheckinRequest(
    val note: String? = null,
    val date: LocalDate? = null  // default: today
)
```

### CreateChallengeDto

```kotlin
data class CreateChallengeDto(
    val title: String,
    val description: String,
    val category: String,
    val icon: String? = null,        // default: "trophy"
    val durationDays: Int? = null,   // default: 21
    val startDate: LocalDate? = null,
    val isPublic: Boolean? = null    // default: true
)
```

## Domain Entities (Server)

### Habit

```kotlin
@Entity @Table(name = "habitflow_habits")
data class Habit(
    val id: String,          // UUID
    val userId: String,
    val title: String,
    val description: String?,
    val icon: String,
    val color: String,
    val category: String,
    val frequency: HabitFrequency,  // DAILY | WEEKLY | CUSTOM
    val weekdays: List<Int>,
    val reminderTime: String?,
    val goalStreak: Int,
    val currentStreak: Int,
    val bestStreak: Int,
    val totalCheckins: Int,
    val status: HabitStatus,        // ACTIVE | ARCHIVED | COMPLETED
    val startDate: LocalDate,
    val createdAt: Instant,
    val updatedAt: Instant
)
```

### HabitCheckin

```kotlin
@Entity @Table(name = "habitflow_checkins")
data class HabitCheckin(
    val id: String,          // UUID
    val habitId: String,
    val userId: String,
    val date: LocalDate,
    val note: String?,
    val photoUrl: String?,
    val aiVerified: Boolean,
    val createdAt: Instant
)
```

### Challenge

```kotlin
@Entity @Table(name = "habitflow_challenges")
data class Challenge(
    val id: String,          // UUID
    val title: String,
    val description: String,
    val category: String,
    val icon: String,
    val durationDays: Int,
    val startDate: LocalDate,
    val endDate: LocalDate,
    val createdBy: String,
    val isPublic: Boolean,
    val participantCount: Int,
    val status: ChallengeStatus  // ACTIVE | COMPLETED | CANCELLED
)
```

### ChallengeParticipant

```kotlin
@Entity @Table(name = "habitflow_challenge_participants")
data class ChallengeParticipant(
    val id: String,          // UUID
    val challengeId: String,
    val userId: String,
    val currentStreak: Int,
    val totalCheckins: Int,
    val joinedAt: Instant
)
```

## JPA Repositories

| Repository                      | Key Queries                                        |
|---------------------------------|----------------------------------------------------|
| `HabitRepository`               | `findByUserIdAndStatus`, `findByUserId`            |
| `HabitCheckinRepository`        | `findByHabitIdAndDate`, `findByUserIdAndDateBetween`, `countByHabitGrouped` |
| `ChallengeRepository`           | `findByIsPublicTrueAndStatus`, `findByCreatedBy`   |
| `ChallengeParticipantRepository`| `findByChallengeIdAndUserId`, `findLeaderboard` (ORDER BY streak DESC) |
