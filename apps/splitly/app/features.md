# Splitly — Feature Spec

- Version: v1.0.5
- Date: 2026-04-08
- Source: Reverse-extracted from `splitly/libs/**` UseCases and `splitly/composeApp/**` Screens/ViewModels

## 핵심 기능 (Features)

| ID    | 영역         | 기능                             | Backing UseCase / Service |
|-------|--------------|----------------------------------|---------------------------|
| F-001 | Split        | 단순 정산 계산                   | `CalculateSimpleSplitUseCase` |
| F-002 | Split        | 고급 정산 (비율/금액/퍼센트)     | `CalculateAdvancedSplitUseCase` |
| F-003 | Split        | 품목별 정산                      | `CalculateItemizedSplitUseCase`, `SaveItemizedSplitUseCase` |
| F-004 | Split        | 영수증 AI 파싱                   | `ParseReceiptWithAiUseCase`, `ReceiptParsingService` |
| F-005 | Settlement   | 부채 최소화(그래프 단순화)       | `DebtSimplificationUseCase` |
| F-006 | Settlement   | 그룹간 부채 상계                 | `CrossGroupBalanceUseCase` |
| F-007 | Settlement   | 지급 계획 저장/조회              | `GetPaymentPlanUseCase`, `SavePaymentPlanUseCase` |
| F-008 | Settlement   | 정산 완료 처리 / 리마인더        | `MarkSettlementCompleteUseCase`, `SendSettlementReminderUseCase`, `CreateSettlementCompletionsUseCase` |
| F-009 | Settlement   | QR 공유 정산                     | `GenerateSettlementQRUseCase` |
| F-010 | Ledger       | 가계부 CRUD                      | `AddTransactionUseCase`, `UpdateTransactionUseCase`, `DeleteTransactionUseCase`, `GetTransactionsUseCase` |
| F-011 | Ledger       | 가계부 통계 (월/카테고리/추세)   | `GetLedgerStatsUseCase` |
| F-012 | Ledger       | Free 트랜잭션 제한               | `CheckTransactionLimitUseCase` |
| F-013 | Insight      | 지출 분석 / 이상 감지            | `SpendingAnalyticsUseCase`, `AnomalyDetectionUseCase` |
| F-014 | Insight      | 월간 리포트 생성                 | `GenerateMonthlyReportUseCase` |
| F-015 | Insight      | 예산 제안                        | `BudgetSuggestionUseCase` |
| F-016 | Friend       | 친구 추가/검색/목록              | `AddFriendUseCase`, `SearchFriendUseCase`, `GetFriendsUseCase` |
| F-017 | Group        | 그룹 초대 / 참여                 | `CreateGroupInviteUseCase`, `GetGroupInviteUseCase`, `JoinGroupUseCase` |
| F-018 | Activity     | 활동 피드                        | `AddActivityUseCase`, `GetActivitiesUseCase` |
| F-019 | Comment      | 정산 댓글                        | `AddCommentUseCase`, `GetCommentsUseCase`, `DeleteCommentUseCase` |
| F-020 | Footprints   | 여행 족적(위치) 기록             | `GetFootprintsUseCase`, `GetFootprintTrailsUseCase`, `GetTripFootprintUseCase`, `GetTripsWithLocationUseCase` |
| F-021 | Export       | 가계부/정산 CSV 내보내기         | `ExportLedgerCsvUseCase`, `ExportSettlementsCsvUseCase` |
| F-022 | Export       | 전체 데이터 JSON 백업/복원       | `ExportAllDataJsonUseCase`, `ImportDataJsonUseCase` |
| F-023 | Currency     | 다통화 표시/환산                 | `FormatCurrencyUseCase` |
| F-024 | Auth         | 로그인 / 로그아웃 / 로컬 동기화  | `SignOutUseCase`, `SyncUserToLocalUseCase` |
| F-025 | Template     | 정산 템플릿                      | `GetTemplatesUseCase` |
| F-026 | Recurring    | 반복 지출                        | `RecurringExpenseRepository` + `RecurringCreate/ListViewModel` |
| F-027 | Feedback     | 사용자 피드백 전송               | `SubmitFeedbackUseCase` |
| F-028 | Paywall      | 프리미엄 구독 / 한도 체크        | `CheckExportLimitUseCase`, `CheckOptimizationLimitUseCase`, `CheckInsightLimitUseCase`, `CheckItemizedSplitLimitUseCase`, `BillingService` |
| F-029 | Security     | 앱 잠금 / 생체 인증              | `AppLockConfig`, `BiometricAuthenticator` |
| F-030 | Data Mgmt    | 계정 삭제 / 데이터 초기화 (v1.0.5) | `DataManagementScreen` |

## 화면 목록

See `screens.md`.

## 데이터 모델 (요약)

```kotlin
// 위치: splitly/composeApp/src/commonMain/kotlin/com/moondeveloper/splitly/domain/model/

data class AuthUser(val id: String, val email: String, val displayName: String?)
data class Currency(val code: String, val symbol: String, val name: String)
data class ExchangeRate(val base: String, val target: String, val rate: Double, val fetchedAt: Long)
data class FriendInfo(val userId: String, val name: String, val avatar: String?)
data class GroupInvite(val inviteCode: String, val groupId: String, val expiresAt: Long)
data class RecurringExpense(val id: String, val title: String, val amount: Double, val period: Period)
data class NotificationPreference(val dailyReminder: Boolean, val settlementAlerts: Boolean)
data class PaymentMethod(val id: String, val type: PaymentApp, val account: String)
enum class PremiumTier { FREE, PRO_MONTHLY, PRO_ANNUAL }
enum class PaywallTriggerType { EXPORT_LIMIT, OPTIMIZATION_LIMIT, INSIGHT_LIMIT, ITEMIZED_LIMIT, TRANSACTION_LIMIT }
data class ActivityEvent(val id: String, val type: String, val actorId: String, val payload: Map<String, Any?>, val createdAt: Long)
data class FootprintItem(val tripId: String, val lat: Double, val lng: Double, val placeName: String?, val visitedAt: Long)
data class TripItem(val id: String, val title: String, val startAt: Long, val endAt: Long?)
data class ShareCard(val id: String, val kind: String, val payload: String)
data class IapConfig(val productIds: List<String>, val monthly: String, val annual: String)
data class AppLockConfig(val enabled: Boolean, val biometric: Boolean, val timeoutSec: Int)
sealed class SplitlyResult<out T> { data class Success<T>(val data: T) : SplitlyResult<T>(); data class Failure(val error: Throwable) : SplitlyResult<Nothing>() }

// insight/
data class InsightCard(val type: InsightType, val title: String, val value: String)
enum class InsightType { TOP_CATEGORY, MONTH_OVER_MONTH, ANOMALY, BUDGET_PROGRESS }
data class MonthlyReport(val yearMonth: String, val total: Double, val byCategory: Map<String, Double>)
data class SpendingAnalytics(val avgDaily: Double, val peakDay: String)
data class SpendingAnomaly(val date: String, val amount: Double, val zScore: Double)
```

## 모듈 구성 (L3)

- `splitly/libs/splitly-ledger` — 가계부 도메인 + UseCase
- `splitly/libs/splitly-settlement` — 정산/분할/부채 도메인 + UseCase
- `splitly/composeApp` — 프레젠테이션 + 잔여 도메인(auth/friend/insight/footprints 등)
