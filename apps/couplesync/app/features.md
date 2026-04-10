# CoupleSync — Feature Spec

- Version: v1.0.0
- Date: 2026-04-10
- Source: Reverse-extracted from `CoupleSync/libs/**` and `CoupleSync/composeApp/**`

## 핵심 기능 (Features)

| ID    | 영역           | 기능                                   | Backing UseCase / Service                                        | Note          |
|-------|----------------|----------------------------------------|------------------------------------------------------------------|---------------|
| F-001 | Pairing        | 커플 초대 코드 생성                    | `PairingEngine.generateInvitation`                               | 24h TTL, 6자리 코드 |
| F-002 | Pairing        | 커플 초대 수락/검증                    | `PairingEngine.validateInvitation`, `PairingEngine.createCouple` |               |
| F-003 | Pairing        | 커플 해소                              | `CoupleSyncService.dissolveCouple` (server)                      |               |
| F-004 | Ledger         | 공유 가계부 CRUD                       | `AddTransactionUseCase`, `UpdateTransactionUseCase`, `DeleteTransactionUseCase`, `GetTransactionsUseCase` | Splitly 공통  |
| F-005 | Ledger         | 가계부 통계 (월/카테고리)              | `GetLedgerStatsUseCase`                                          | Splitly 공통  |
| F-006 | Ledger         | Free 트랜잭션 제한                     | `CheckTransactionLimitUseCase`                                   | Splitly 공통  |
| F-007 | Settlement     | 커플 정산 (단순/고급/품목별)           | `CalculateSimpleSplitUseCase`, `CalculateAdvancedSplitUseCase`, `CalculateItemizedSplitUseCase` | Splitly 공통  |
| F-008 | Settlement     | 부채 최소화                            | `DebtSimplificationUseCase`                                      | Splitly 공통  |
| F-009 | Settlement     | 그룹간 부채 상계                       | `CrossGroupBalanceUseCase`                                       | Splitly 공통  |
| F-010 | Settlement     | 지급 계획 저장/조회                    | `GetPaymentPlanUseCase`, `SavePaymentPlanUseCase`                | Splitly 공통  |
| F-011 | Settlement     | 정산 완료/조회                         | `GetPendingSettlementsUseCase`, `GetSettlementCompletionsUseCase` | Splitly 공통  |
| F-012 | Settlement     | QR 공유 정산                           | `GenerateSettlementQRUseCase`                                    | Splitly 공통  |
| F-013 | Settlement     | 정산 삭제                              | `DeleteSettlementsUseCase`                                       | Splitly 공통  |
| F-014 | Settlement     | 정산 템플릿                            | `GetTemplatesUseCase`                                            | Splitly 공통  |
| F-015 | Settlement     | 정산 규칙 (자동 정산)                  | `AutoSettlementEngine`, `SettlementRuleRepository`               | 카테고리/월별 트리거 |
| F-016 | Settlement     | 커플 정산 엔진                         | `CoupleSettlementEngine`                                         |               |
| F-017 | Settlement     | 공유 정산 링크                         | `SharedSettlementLink`                                           | 7일 만료      |
| F-018 | Goal           | 커플 저축 목표 CRUD                    | `GoalRepository`, `GoalViewModel`                                |               |
| F-019 | Goal           | 목표 입금/완료                         | `GoalDeposit`, `GoalStatus`                                      |               |
| F-020 | Goal           | 목표 템플릿                            | `GoalTemplate`                                                   |               |
| F-021 | Goal           | 목표 한도 체크 (플랜별)                | `GoalLimitChecker`                                               | FREE=0, STD=3, PRO=무제한 |
| F-022 | Calendar       | 커플 공유 캘린더 CRUD                  | `CalendarRepository`, `CalendarViewModel`                        |               |
| F-023 | Calendar       | Google Calendar 동기화                 | `GoogleCalendarSyncManager`                                      |               |
| F-024 | Anniversary    | 기념일 관리 + D-Day 계산               | `DdayCalculator`, `AnniversaryAutoGenerator`                     | 100/200/300/500/1000일 자동 생성 |
| F-025 | Anniversary    | 기념일 카드 생성/공유                  | `AnniversaryCardViewModel`, `AnniversaryCard`                    | 4종 템플릿 (ROMANTIC/ELEGANT/WARM/MINIMAL) |
| F-026 | Memory         | 추억 기록 (포토 다이어리)              | `MemoryRepository`, `MemoryViewModel`                            | 해시태그, 위치, 타임라인/그리드/히트맵 뷰 |
| F-027 | Memory         | 월간 슬라이드쇼                        | `GetMonthlySlideshowUseCase`                                     |               |
| F-028 | Memory         | 연간 리뷰                              | `GetYearlyReviewUseCase`                                         | 지출/수입/데이트수/추억수/저축률 종합 |
| F-029 | DatePlan       | 데이트 계획 관리                       | `DatePlanRepository`, `DatePlanViewModel`                        | 장소 목록, 예산, 상태 관리 |
| F-030 | Wishlist       | 커플 위시리스트 (상호 투표)            | `WishlistRepository`                                             | 양쪽 동의 시 `isMutuallyWanted` |
| F-031 | Challenge      | 커플 챌린지 (30일)                     | `ChallengeRepository`, `ChallengeViewModel`                      | 5종 (칭찬/감사/새로운것/데이트/절약) |
| F-032 | Emotion        | 감정 체크인 (일 1회)                   | `EmotionRepository`, `EmotionViewModel`                          | 5종 감정 (HAPPY/LOVING/NEUTRAL/TIRED/SAD) |
| F-033 | Budget         | 카테고리별 예산 관리                   | `BudgetRepository`                                               |               |
| F-034 | Report         | 월간 리포트 (재무/활동/감정)           | `MonthlyReportGenerator`, `MonthlyReportViewModel`               | 기여도 비율 포함 |
| F-035 | AI             | AI 데이트 추천 (Gemini)                | `GeminiRepository.recommendDates`, `DateRecommendViewModel`      |               |
| F-036 | AI             | AI 선물 추천 (Gemini)                  | `GeminiRepository.recommendGifts`, `GiftRecommendViewModel`      |               |
| F-037 | AI             | AI 지출 인사이트 (Gemini)              | `GeminiRepository.analyzeSpending`, `SpendingInsightViewModel`   |               |
| F-038 | Notification   | 파트너 넛지 / 리마인더                 | `NudgeEngine`, `PushNotificationManager`                         | 가계부 입력 리마인더, 기념일 알림 |
| F-039 | Sync           | 커플 데이터 실시간 동기화              | `CoupleSyncEngine`                                               | 오프라인 → 온라인 충돌 해결 |
| F-040 | Sync           | WebSocket 실시간 알림                  | `CoupleSyncWebSocketController` (server)                         | 파트너 온/오프라인, CRUD 이벤트 |
| F-041 | Auth           | 로그인/로그아웃                        | `AuthViewModel`                                                  | Splitly 공통  |
| F-042 | Billing        | 프리미엄 구독 (FREE/STANDARD/PRO)      | `CoupleSubscriptionStateManager`, `CoupleSubscriptionPlan`       | STD: 4,900원/월, PRO: 7,900원/월 |
| F-043 | Theme          | 커플 테마 변경                         | `ThemeRepository`, `ThemeViewModel`                              | 2 Free + 4 Premium 테마 |
| F-044 | Security       | 앱 잠금 (PIN)                          | `AppLockManager`                                                 |               |
| F-045 | Security       | 프라이버시 노트                        | `PrivacyNote`, `PrivacySpaceViewModel`                           | 개인 비공개 메모 |
| F-046 | Ad             | 광고 (Free 플랜)                       | `AdService`, `AdMobBanner`                                       |               |

## 커플 페어링 플로우 (Couple Pairing Flow)

```
User A                          Server                          User B
  |                               |                               |
  |-- POST /invitations --------->|                               |
  |<-- { code: "ABC123" } --------|                               |
  |                               |                               |
  |  (Share code via messenger)   |                               |
  |                               |                               |
  |                               |<-- GET /invitations/{code}/preview -- |
  |                               |-- { valid: true } ----------->|
  |                               |                               |
  |                               |<-- POST /invitations/{code}/accept --|
  |                               |-- { pairId: "..." } -------->|
  |                               |                               |
  |<-- WS: PARTNER_ONLINE --------|-- WS: PARTNER_ONLINE ------->|
```

- Invitation code: 6 chars (A-Z excluding I/O, 2-9), 24-hour TTL
- Validation: self-pairing blocked, expired/used codes rejected
- On accept: `CouplePair` created, WebSocket connection established
- Dissolution: `DELETE /couples/me` sets status to `DISSOLVED`

## 데이터 모델 (요약)

```kotlin
// couplesync-couple (L3)
data class Couple(pairId, user1Uid, user2Uid, anniversaryDate, status: CoupleStatus, createdAt)
data class PairInvitation(code, creatorUid, expiresAt, status: InvitationStatus)
data class Anniversary(id, pairId, name, date, repeatType: RepeatType, reminderDays)
data class Memory(id, pairId, ownerUid, title, memo, photoUrls, location, date, hashtags, updatedAt)
data class DatePlan(id, pairId, title, date, budget, places: List<DatePlace>, status, memo, updatedAt)
data class DatePlace(id, name, category: PlaceCategory, address, memo, estimatedCost, order)

// couplesync-ledger (L3)
data class LedgerTransaction(id, type, amount, title, category: LedgerCategory, date, memo, currencyCode, lat/lng, locationName, linkedSettlementId, createdAt, updatedAt)
data class LedgerStats(...)
data class MonthlySummary(...)
data class Budget(id, pairId, category, monthlyLimit, yearMonth, updatedAt)

// couplesync-settlement (L3)
data class Settlement(...)
data class Debt(...)
data class PaymentPlan(...)
data class SettlementRule(id, pairId, name, defaultRatio, triggerType: RuleTriggerType, dayOfMonth, category, isActive, updatedAt)
data class SharedSettlementLink(shareId, pairId, settlementId, totalAmount, ratio, createdAt, expiresAt)

// couplesync-goal (L3)
data class SavingGoal(id, pairId, name, template: GoalTemplate, targetAmount, currentAmount, targetDate, user1Ratio, priority, isCompleted, updatedAt)
data class GoalDeposit(...)

// composeApp domain models
data class CalendarEvent(id, pairId, ownerUid, title, startAt, endAt, location, color: EventColor, inviteStatus, updatedAt)
data class CoupleChallenge(id, pairId, type: ChallengeType, startDate, targetDays, status: ChallengeStatus, completedAt)
data class ChallengeLog(id, challengeId, pairId, ownerUid, date, memo, createdAt)
data class EmotionCheckIn(id, pairId, ownerUid, emotion: EmotionType, memo, date, createdAt)
data class WishlistItem(id, pairId, name, category: WishlistCategory, url, price, vote1, vote2, isPurchased, updatedAt)
data class MonthlyReport(pairId, yearMonth, finance: FinanceSummary, activity: ActivitySummary, emotion: EmotionSummary)
data class AnniversaryCard(anniversaryName, dayCount, photoUrl, message, template: CardTemplate)
data class PrivacyNote(id, uid, content, createdAt, updatedAt)
enum class CoupleTheme { ROMANTIC, MODERN, NATURAL(Pro), VIVID(Pro), DARK(Pro), SEASON(Pro) }
```

## 모듈 구성 (L3)

| Module                   | Role                                                      |
|--------------------------|-----------------------------------------------------------|
| `couplesync-auth`        | Firebase Auth 추상화 (expect/actual per platform)          |
| `couplesync-billing`     | 구독 플랜 (FREE/STANDARD/PRO), 구매 상태 관리              |
| `couplesync-couple`      | 페어링, 기념일, 추억, 데이트플랜, D-Day, Gemini AI 도메인   |
| `couplesync-goal`        | 저축 목표 도메인 + 한도 체크                                |
| `couplesync-ledger`      | 공유 가계부 도메인 + UseCase (Splitly 공통 구조)            |
| `couplesync-settlement`  | 정산/분할/부채/QR 도메인 + UseCase (Splitly 공통 구조)      |
| `couplesync-sync`        | 오프라인-온라인 동기화 엔진, 충돌 해결                      |
| `couplesync-ui`          | 커플 전용 테마 토큰 + 컬러 시스템                           |
| `composeApp`             | 프레젠테이션 + 잔여 도메인 (캘린더, 챌린지, 감정, 위시리스트, 예산, 보안, 알림, 광고) |
