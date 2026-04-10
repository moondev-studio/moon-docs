# FocusOn — Feature Spec

- Version: v1.0.0
- Date: 2026-04-10
- Source: Reverse-extracted from `FocusOn/libs/**` and `FocusOn/composeApp/**`

## 핵심 기능 (Features)

| ID    | 영역           | 기능                                        | Backing UseCase / Service                                   |
|-------|----------------|---------------------------------------------|-------------------------------------------------------------|
| F-001 | Timer          | 포모도로 타이머 (25/5/15 cycle)             | `TimerEngine`, `TimerConfig`, `TimerState`                  |
| F-002 | Timer          | 스톱워치 모드                               | `TimerEngine` (mode=STOPWATCH)                              |
| F-003 | Timer          | 페이즈 스킵 (집중/휴식 전환)                | `TimerEngine.skipPhase()`                                   |
| F-004 | Soundscape     | 배경음 재생 (Rain/Cafe/Library/Nature/Piano) | `SoundscapeManager`, `Soundscape` enum                      |
| F-005 | Sound Mixer    | 다중 사운드 레이어 믹싱                     | `SoundMixerEngine`, `SoundLayer`, `SoundPreset`             |
| F-006 | Sound Mixer    | 프리셋 저장/적용                            | `SoundMixerEngine`, `SoundPreset`                           |
| F-007 | Sound          | 프리미엄 사운드 팩                          | `PremiumSoundCatalog`, `PremiumSound`                       |
| F-008 | Session        | 집중 세션 저장 (로컬 + 서버 동기화)         | `SaveFocusSessionUseCase`, `FocusSessionDao`                |
| F-009 | Statistics     | 일간/주간/월간/연간 통계                    | `StatisticsRepository`, `StatisticsViewModel`               |
| F-010 | Statistics     | 연속 학습일(스트릭) 추적                    | `StreakInfo`, `StatisticsViewModel`                         |
| F-011 | Statistics     | 과목별 학습 분포                            | `StatisticsViewModel` (subjectDistribution)                 |
| F-012 | Goal           | 일일 목표 설정/달성 추적                    | `GoalRepository`, `GoalDao`                                 |
| F-013 | Achievement    | 업적 시스템 (9종)                           | `CheckAndGrantAchievementsUseCase`, `AchievementDao`        |
| F-014 | Subject        | 과목 관리 (CRUD)                            | `SubjectRepository`, `SubjectDao`                           |
| F-015 | Auth           | Google/Apple/Kakao 소셜 로그인              | `SignInWithGoogleUseCase`, `SignInWithAppleUseCase`, `KakaoLoginUseCase` |
| F-016 | Auth           | 프로필 편집 / 계정 삭제                     | `UpdateProfileUseCase`, `DeleteAccountUseCase`              |
| F-017 | Onboarding     | 온보딩 위저드 (목표 설정 포함)              | `OnboardingViewModel`, `AppSettingsRepository`              |
| F-018 | Social         | 친구 추가/검색/요청/수락                    | `SendFriendRequestUseCase`, `AcceptFriendRequestUseCase`, `SearchUserUseCase` |
| F-019 | Social         | 친구 랭킹 / 글로벌 랭킹                    | `GetFriendRankingUseCase`, `GetGlobalRankingUseCase`        |
| F-020 | Social         | 찌르기(Nudge) 알림                          | `SendNudgeUseCase`, `FcmService`                            |
| F-021 | Study Room     | 스터디룸 생성/참여/나가기                   | `CreateRoomUseCase`, `JoinRoomUseCase`, `LeaveRoomUseCase`  |
| F-022 | Study Room     | 코드 초대 참여                              | `JoinRoomByCodeUseCase`                                     |
| F-023 | Study Room     | 실시간 집중 상태 동기화 (WebSocket)         | `UpdateFocusStatusUseCase`, `StudyRoomWebSocketController`  |
| F-024 | Study Room     | 주간 그룹 목표 / 그룹 뱃지                  | `CheckGroupGoalUseCase`, `GroupBadge`                       |
| F-025 | Planner        | D-day 시험 플래너                           | `DdayPlannerEngine`, `AddExamUseCase`, `GetTodayPlansUseCase` |
| F-026 | Event          | 라이브 이벤트 참여/랭킹                     | `JoinEventUseCase`, `GetLiveEventsUseCase`, `LiveRankingEngine` |
| F-027 | Event          | 타이머 집중 시간 이벤트 연동                | `AddEventFocusUseCase`                                      |
| F-028 | Share          | 공유 카드 생성/공유                         | `BuildShareCardUseCase`, `ShareCardEngine`, `ShareCardRenderer` |
| F-029 | AI             | AI 피크 타임 분석                           | `AnalyzePeakTimeUseCase`, `AiRepository`                    |
| F-030 | AI             | AI 스트릭 위험 예측                         | `PredictStreakRiskUseCase`, `AiRepository`                   |
| F-031 | AI             | AI 코칭 챗봇                                | `GetAiCoachingUseCase`, `AiRepository`                      |
| F-032 | AI             | 연간 리뷰 생성/공유                         | `GenerateYearReviewUseCase`, `AiRepository`                 |
| F-033 | AI             | AI 크레딧 관리                              | `AiCreditEngine`, `AiCreditRepository`, `CheckAiCreditUseCase` |
| F-034 | Billing        | 구독 관리 (FREE/STANDARD/PRO)               | `PurchaseStandardUseCase`, `PurchaseProUseCase`, `RestorePurchasesUseCase` |
| F-035 | Billing        | 페이월 (기능 제한 트리거)                   | `CheckPlanUseCase`, `GetSubscriptionStateUseCase`           |
| F-036 | Ad             | 배너 광고 (타이머 중 미노출 정책, commit 14dfc7d) | `AdMobBanner`, `AdTimingPolicy.isBannerAllowed()`     |
| F-037 | Ad             | 전면 광고 (15분 이상 세션 완료 시)          | `InterstitialAdManager`, `AdTimingPolicy.isInterstitialAllowed()` |
| F-038 | B2B            | 조직 대시보드 / 팀 관리                     | `GetOrgStatsUseCase`, `CreateTeamUseCase`, `B2BRepository`  |
| F-039 | B2B            | SSO 인증 설정                               | `SsoAuthProvider`, `SsoConfig`                              |
| F-040 | Sync           | Firestore 원격 동기화                       | `FirestoreRemoteStore`, `focuson-sync` module               |
| F-041 | Analytics      | Firebase Analytics / Crash / Performance    | `focuson-analytics` module                                  |
| F-042 | i18n           | 다국어 지원                                 | `focuson-i18n` module, `LocaleManager`                      |
| F-043 | Watch          | Watch 연동 브릿지                           | `WatchBridge`                                               |
| F-044 | TimeLapse      | 타임랩스 스냅샷                             | `TimeLapseEngine`, `TimeLapseSnapshot`                      |

## Known Issues

- **InterstitialAdManager session flow connection incomplete**: `InterstitialAdManager` (Android-only) is implemented as a standalone utility class but is not wired into the `TimerViewModel` session-complete flow. The `AdTimingPolicy` validates timing rules correctly (covered by `InterstitialAdSessionTest` and `AdTimingPolicyExtendedTest` using fakes), but actual invocation from the presentation layer after a focus session ends is missing. iOS side has `IOSInterstitialAdBridge` but similarly lacks integration.
- **Banner ad timing policy** (commit 14dfc7d): Banner ads are NEVER shown during active or paused timer sessions. `AdTimingPolicy.isBannerAllowed()` returns `false` when `isRunning=true` or when paused mid-session. Only idle and result screens display banner ads. This is enforced at the `AdMobBanner` composable level with a test tag (`BANNER_AD_TEST_TAG`) for UI test verification.

## 데이터 모델 (요약)

```kotlin
// Local DB — Room KMP (FocusOnDatabase)
@Entity("focus_sessions")
data class FocusSessionEntity(id, userId, subjectId, startTime, endTime, durationSeconds, type, soundscape, completed, synced)

@Entity("achievements")
data class AchievementEntity(id, userId, type: AchievementType, unlockedAt)
// AchievementType: FIRST_SESSION, STREAK_3/7/30, TOTAL_10H/100H, SUBJECT_5, EARLY_BIRD, NIGHT_OWL

@Entity("goals")
data class GoalEntity(id, userId, type: GoalType, targetMinutes, ...)

@Entity("subjects")
data class SubjectEntity(id, userId, name, color, ...)

@Entity("dday_exams")
data class DdayExamEntity(id, userId, examName, examDate, totalStudyHours, ...)

@Entity("study_rooms")
data class StudyRoomEntity(id, ownerId, name, ...)

@Entity("friends")
data class FriendEntity(id, userId, friendId, ...)

@Entity("app_settings")
data class AppSettingsEntity(id, onboardingCompleted, ...)

@Entity("ai_insights")
data class AIInsightEntity(id, userId, type, content, ...)

// Timer (focuson-timer lib)
data class TimerConfig(focusMinutes=25, shortBreakMinutes=5, longBreakMinutes=15, cyclesUntilLongBreak=4, mode: TimerMode)
enum class TimerMode { POMODORO, STOPWATCH }
enum class TimerPhase { FOCUS, SHORT_BREAK, LONG_BREAK }
data class TimerState(phase, mode, remainingSeconds, elapsedSeconds, completedCycles, isRunning, startedAtMillis, subjectId)

// Domain models
enum class SubscriptionPlan { FREE, STANDARD, PRO }
data class SubscriptionState(plan, expiresAt, isTrialActive)
sealed class AuthState { Loading, Unauthenticated, Authenticated(user) }
data class StreakInfo(currentStreak, longestStreak, isTodayComplete)
```

## 모듈 구성 (L3)

| Module | Description |
|--------|-------------|
| `focuson-timer` | 타이머 엔진 (TimerEngine, TimerConfig, TimerState, SoundscapeManager) — expect/actual per platform |
| `focuson-auth` | Firebase 인증 (FirebaseAuthProvider) — expect/actual per platform |
| `focuson-billing` | 구독/결제 (FocusOnBillingModule) — expect/actual per platform |
| `focuson-analytics` | Analytics/Crash/Performance (Firebase) — expect/actual per platform |
| `focuson-sync` | Firestore 동기화 (FirestoreRemoteStore, NetworkMonitor) — expect/actual per platform |
| `focuson-i18n` | 다국어 (LocaleManager) — expect/actual per platform |
| `focuson-ui` | 공유 UI 테마 (Color, Type, Dimens, FocusOnColorTokens) |
| `composeApp` | 프레젠테이션 + 잔여 도메인 (AI, B2B, social, event, planner, mixer, share, timelapse, watch) |
