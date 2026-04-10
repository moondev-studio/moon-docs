# HomeTasks — Feature Spec

- Version: v1.0.0
- Date: 2026-04-10
- Status: All features are **planned** (design-first, no implementation yet)

## Core Features

| ID    | Area              | Feature                                    | Status  | Backing UseCase / Service (planned) |
|-------|-------------------|--------------------------------------------|---------|-------------------------------------|
| F-001 | Task              | Task CRUD (create, read, update, delete)   | planned | `CreateTaskUseCase`, `UpdateTaskUseCase`, `DeleteTaskUseCase`, `GetTasksUseCase` |
| F-002 | Task              | Task categories (kitchen, bathroom, etc.)  | planned | `GetCategoriesUseCase`, `AssignCategoryUseCase` |
| F-003 | Task              | Estimated time per task                    | planned | `SetEstimatedTimeUseCase` |
| F-004 | Task              | Assignee designation                       | planned | `AssignTaskUseCase` |
| F-005 | Task              | Recurring task schedule (daily/weekly/custom) | planned | `SetRecurrenceUseCase`, `RecurrenceScheduler` |
| F-006 | Task              | Task priority (high/medium/low)            | planned | `SetTaskPriorityUseCase` |
| F-007 | Member            | Invite member via link/code                | planned | `CreateInviteLinkUseCase`, `JoinTeamUseCase` |
| F-008 | Member            | Team member list & roles                   | planned | `GetTeamMembersUseCase`, `UpdateMemberRoleUseCase` |
| F-009 | Member            | Remove member                              | planned | `RemoveMemberUseCase` |
| F-010 | Rotation          | Auto-rotation schedule (round-robin)       | planned | `GenerateRotationUseCase`, `GetRotationScheduleUseCase` |
| F-011 | Rotation          | Custom rotation rules (skip days, preferences) | planned | `SetRotationPreferenceUseCase` |
| F-012 | Rotation          | Swap request between members               | planned | `RequestSwapUseCase`, `ApproveSwapUseCase` |
| F-013 | Completion        | Mark task as done (timestamp)              | planned | `CompleteTaskUseCase` |
| F-014 | Completion        | Photo proof of completion                  | planned | `UploadCompletionPhotoUseCase` |
| F-015 | Completion        | Undo completion                            | planned | `UndoCompletionUseCase` |
| F-016 | Stats             | Per-member completion rate                 | planned | `GetCompletionStatsUseCase` |
| F-017 | Stats             | Workload distribution chart                | planned | `GetWorkloadDistributionUseCase` |
| F-018 | Stats             | Weekly summary report                      | planned | `GenerateWeeklyReportUseCase` |
| F-019 | Stats             | Monthly trend analysis                     | planned | `GetMonthlyTrendUseCase` |
| F-020 | Reward            | Points earned per task completion           | planned | `AwardPointsUseCase`, `GetPointsBalanceUseCase` |
| F-021 | Reward            | Weekly MVP badge                           | planned | `CalculateMvpUseCase`, `AwardBadgeUseCase` |
| F-022 | Reward            | Streak tracking (consecutive days)         | planned | `UpdateStreakUseCase`, `GetStreakUseCase` |
| F-023 | Reward            | Reward history log                         | planned | `GetRewardHistoryUseCase` |
| F-024 | Auth              | Login / logout / profile                   | planned | `SignInUseCase`, `SignOutUseCase`, `GetProfileUseCase` |
| F-025 | Notification      | Task reminder push notification            | planned | `ScheduleReminderUseCase`, `PushNotificationService` |
| F-026 | Notification      | Completion notification to team            | planned | `NotifyCompletionUseCase` |
| F-027 | Paywall           | Premium subscription / limit check         | planned | `CheckMemberLimitUseCase`, `CheckRewardAccessUseCase`, `BillingService` |
| F-028 | Settings          | Notification preferences                   | planned | `UpdateNotificationPrefUseCase` |
| F-029 | Settings          | Language selection                         | planned | `SetLanguageUseCase` |
| F-030 | Feedback          | User feedback submission                   | planned | `SubmitFeedbackUseCase` |

## Data Model (planned)

```kotlin
// planned location: HomeTasks/libs/hometasks-domain/src/commonMain/kotlin/.../model/

data class HouseTask(
    val id: String,
    val title: String,
    val category: TaskCategory,
    val estimatedMinutes: Int,
    val priority: Priority,
    val assigneeId: String?,
    val recurrence: Recurrence?,
    val createdAt: Long,
    val updatedAt: Long
)

enum class TaskCategory { KITCHEN, BATHROOM, LIVING_ROOM, BEDROOM, LAUNDRY, OUTDOOR, GROCERY, OTHER }
enum class Priority { HIGH, MEDIUM, LOW }

data class Recurrence(val type: RecurrenceType, val interval: Int, val daysOfWeek: List<Int>?)
enum class RecurrenceType { DAILY, WEEKLY, BIWEEKLY, MONTHLY, CUSTOM }

data class TaskCompletion(
    val id: String,
    val taskId: String,
    val memberId: String,
    val completedAt: Long,
    val photoUrl: String?
)

data class TeamMember(
    val id: String,
    val userId: String,
    val displayName: String,
    val role: MemberRole,
    val joinedAt: Long
)
enum class MemberRole { ADMIN, MEMBER }

data class RotationSchedule(
    val taskId: String,
    val memberOrder: List<String>,
    val currentIndex: Int,
    val cadence: Recurrence
)

data class RewardPoints(
    val memberId: String,
    val totalPoints: Int,
    val weeklyPoints: Int,
    val currentStreak: Int,
    val bestStreak: Int
)

data class Badge(val id: String, val type: BadgeType, val awardedAt: Long)
enum class BadgeType { WEEKLY_MVP, STREAK_7, STREAK_30, FIRST_TASK, HUNDRED_TASKS }
```

## Module Structure (planned, L3)

- `HomeTasks/libs/hometasks-domain` — Domain models, UseCases
- `HomeTasks/libs/hometasks-feature-tasks` — Task list, add, detail screens
- `HomeTasks/libs/hometasks-feature-team` — Team setup, dashboard, invite
- `HomeTasks/libs/hometasks-feature-stats` — Stats and reward screens
- `HomeTasks/composeApp` — Presentation layer, navigation, DI wiring
