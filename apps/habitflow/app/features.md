# HabitFlow — Feature Spec

- Version: v1.0.0
- Date: 2026-04-10
- Source: Reverse-extracted from `HabitFlow/libs/**` and `HabitFlow/composeApp/**`

## 핵심 기능 (Features)

| ID    | 영역            | 기능                                     | Backing UseCase / Service                        |
|-------|-----------------|------------------------------------------|--------------------------------------------------|
| F-001 | Habit           | Habit CRUD (create/update/archive/delete)| `HabitEngine.createHabit`, `updateHabit`, `archiveHabit`, `deleteHabit` |
| F-002 | Habit           | Template-based habit creation (30 templates) | `HabitTemplates`, `HabitCreateViewModel`      |
| F-003 | Habit           | Daily check-in / undo check-in           | `CheckInHabitUseCase`, `HabitEngine.checkIn`, `cancelCheckIn`, `toggleCompletion` |
| F-004 | Habit           | Freeze day (streak preservation)         | `HabitEngine.useFreezeDay`, `HabitEngine.canUseFreeze` |
| F-005 | Habit           | Habit filtering by day-of-week           | `HabitEngine.filterHabitsForToday`               |
| F-006 | Streak          | Streak tracking (current/longest/total)  | `HabitEngine.recalcStreak`, `HabitEngine.incrementStreak` |
| F-007 | Streak          | Milestone detection (7/14/30/50/100/365) | `HabitEngine.getMilestone`                       |
| F-008 | Statistics      | Weekly completion stats (7-day trend)    | `StatisticsEngine.observeWeeklyStats`, `StatisticsRepository` |
| F-009 | Statistics      | Monthly heatmap                          | `StatisticsEngine.getMonthlyHeatmap`, `StatisticsRepository` |
| F-010 | Statistics      | Per-habit analytics (rate/streak detail) | `StatisticsEngine.getHabitStats`, `StatisticsRepository` |
| F-011 | Statistics      | Daily completion rate calculation        | `HabitEngine.calcCompletionRate`                 |
| F-012 | Auth            | Email/password sign-in/sign-up           | `HabitFlowAuthRepository.signInWithEmail`, `signUpWithEmail` |
| F-013 | Auth            | Google sign-in                           | `HabitFlowAuthRepository.signInWithGoogle`        |
| F-014 | Auth            | Apple sign-in                            | `HabitFlowAuthRepository.signInWithApple`         |
| F-015 | Auth            | Sign-out / account deletion              | `HabitFlowAuthRepository.signOut`, `deleteAccount` |
| F-016 | Auth            | Password reset                           | `HabitFlowAuthRepository.sendPasswordResetEmail`  |
| F-017 | Billing         | Subscription management (FREE/PRO)       | `HabitFlowBillingService`, `BillingRepository`   |
| F-018 | Billing         | Product query / purchase / restore       | `HabitFlowBillingService.queryProducts`, `purchase`, `restorePurchases` |
| F-019 | Billing         | Habit limit enforcement (FREE: 5, PRO: unlimited) | `BillingProducts`, `HabitEngine.getActiveHabitCount` |
| F-020 | Sync            | Cloud sync (push habit/record, pull all) | `SyncUseCase`, `HabitFlowSyncService`, `SyncRepository` |
| F-021 | Sync            | Offline queue with network monitoring    | `HabitFlowSyncService.enqueueHabitChange`, `NetworkMonitor` |
| F-022 | Profile         | User profile CRUD (avatar, theme, lang)  | `UserProfileRepository`                          |
| F-023 | Settings        | Theme selection (Light/Dark/System)      | `SettingsViewModel`, `UserProfile.theme`         |
| F-024 | Settings        | Notification preferences                 | `UserProfile.notificationsEnabled`               |
| F-025 | Challenge       | Public challenge listing / detail        | `ChallengeEngine` (stub), server `HabitFlowService.getChallenges` |
| F-026 | Challenge       | Join/leave challenge                     | Server `HabitFlowService.joinChallenge`, `leaveChallenge` |
| F-027 | Challenge       | Challenge leaderboard                    | Server `HabitFlowService.getLeaderboard`         |
| F-028 | Challenge       | Create custom challenge                  | Server `HabitFlowService.createChallenge`        |
| F-029 | AI              | AI-powered features (planned)            | `HabitFlowAi` (stub)                             |
| F-030 | Ads             | AdMob banner integration                 | `AdMobBanner`, `AdConfig`                        |
| F-031 | Widget          | Home screen widget updates               | `WidgetUpdater`                                  |

## 화면 목록

See `screens.md`.

## 데이터 모델 (요약)

```kotlin
// Location: HabitFlow/composeApp/src/commonMain/kotlin/.../domain/model/

data class Habit(
    val id: String, val userId: String, val title: String, val description: String,
    val iconEmoji: String, val colorHex: String, val category: HabitCategory,
    val frequency: HabitFrequency, val targetDays: List<Int>,
    val reminderTimes: List<LocalTime>, val goalType: GoalType, val goalValue: Float,
    val goalUnit: String, val freezeDaysUsed: Int, val freezeDaysMax: Int,
    val isActive: Boolean, val createdAt: Long, val order: Int
)
enum class HabitCategory { EXERCISE, LEARNING, HEALTH, LIFESTYLE, PRODUCTIVITY, FINANCE, CUSTOM }
enum class HabitFrequency { DAILY, WEEKLY, CUSTOM }
enum class GoalType { BINARY, QUANTITY }

data class HabitRecord(
    val id: String, val habitId: String, val userId: String, val date: Long,
    val completedAt: Long?, val value: Float, val memo: String,
    val photoPath: String?, val isFreezed: Boolean
)

data class Streak(
    val habitId: String, val userId: String, val currentStreak: Int,
    val longestStreak: Int, val lastCompletedDate: Long?,
    val totalCompletedDays: Int, val updatedAt: Long
)

data class HabitTemplate(
    val id: String, val title: String, val description: String,
    val category: HabitCategory, val iconEmoji: String, val colorHex: String,
    val goalType: GoalType, val goalValue: Float, val goalUnit: String,
    val defaultTargetDays: List<Int>
)

data class DailyStats(val date: String, val completedCount: Int, val totalCount: Int)
data class WeeklyStats(val completionByDay: List<DailyStats>, val averageCompletionRate: Float)
data class HeatmapDay(val date: String, val completedCount: Int, val intensity: Float)
data class HabitStatsDetail(val habitId: String, val title: String, val currentStreak: Int, val longestStreak: Int, val totalCompletedDays: Int, val completionRate: Float)

data class UserProfile(
    val userId: String, val displayName: String, val bio: String,
    val avatarEmoji: String, val notificationsEnabled: Boolean,
    val language: String, val theme: AppTheme, val createdAt: Long, val updatedAt: Long
)
enum class AppTheme { LIGHT, DARK, SYSTEM }

enum class HabitPlan { FREE, PRO }
data class SubscriptionState(
    val plan: HabitPlan, val isTrialActive: Boolean, val trialEndDate: Long?,
    val expiryDate: Long?, val productId: String?
)
object BillingProducts {
    const val PRO_MONTHLY = "habitflow_pro_monthly"
    const val PRO_YEARLY = "habitflow_pro_yearly"
    const val FREE_HABIT_LIMIT = 5
    const val PRO_FREEZE_MONTHLY = 5
    const val FREE_FREEZE_MONTHLY = 1
}

// Auth (from habitflow-auth lib)
data class AuthUser(val uid: String, val email: String?, val displayName: String?, val photoUrl: String?)
sealed class AuthResult<out T> { ... }
enum class AuthProvider { EMAIL, GOOGLE, APPLE }
```

## 모듈 구성 (L3)

- `HabitFlow/libs/habitflow-habit-engine` — Habit CRUD, completion tracking, streak management, statistics engine (Room DAOs)
- `HabitFlow/libs/habitflow-auth` — Authentication repository contract + Firebase/NoOp implementations (expect/actual)
- `HabitFlow/libs/habitflow-billing` — Billing facade wrapping OSS `moon-billing-kmp` (BillingEngine, PremiumStateManager)
- `HabitFlow/libs/habitflow-sync` — Sync facade wrapping OSS `moon-sync-kmp` (SyncManager, NetworkMonitor)
- `HabitFlow/libs/habitflow-challenge-engine` — Challenge engine (stub, TBD)
- `HabitFlow/libs/habitflow-ai` — AI-powered features (stub, TBD)
- `HabitFlow/composeApp` — Presentation layer (screens, ViewModels, navigation) + app-level domain (UseCases, repositories)
