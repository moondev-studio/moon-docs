# HomeTasks — Product Overview
> Version: 1.0.0 | Updated: 2026-04-08 | Status: 개발 진행중
> Work Plan: [70] / [78] / [93] | Ref: HomeTasks-Product-Spec (MCP)

## 제품 개요
| 항목 | 내용 |
|------|------|
| 슬로건 | "함께 사는 우리, 집안일도 함께" |
| 타겟 | 커플, 룸메이트, 가족 (2인 이상 공동생활) |
| 플랫폼 | Android, iOS, Web, 위젯, Dynamic Island / Now Bar |
| 번들 ID | com.moondeveloper.hometasks |
| 수익 | AdMob (Free) + 구독 ₩2,900/월 / ₩22,900/년 |

## 현재 구현 완료 (Work Plan #78)
- assembleDebug PASS (commit 3962e0b)
- 태스크 CRUD UI
  - `libs/hometasks-feature-tasks/.../TaskListScreen.kt`
  - `libs/hometasks-feature-tasks/.../AddTaskScreen.kt`
  - `libs/hometasks-feature-tasks/.../TaskDetailScreen.kt`
- 팀 화면 Stub
  - `libs/hometasks-feature-team/.../TeamSetupScreen.kt`
  - `libs/hometasks-feature-team/.../TeamDashboardScreen.kt`
- moon-app-libs stub 연결
- navigation-compose 추가 (`composeApp/.../navigation/Screen.kt`)

### 코드 상태 (실측)
- 5개 Feature Screen 구현, UseCase 레이어 아직 없음 → #93에서 추가 예정

## 핵심 기능
1. 집안일 관리 (카테고리, 소요 시간, 담당자 배정, 반복 스케줄)
2. 팀 관리 (Free 2명, Premium 무제한) — CoupleSync 재사용
3. 보상 시스템 (포인트, 주간 MVP 뱃지, 스트릭)
4. 통계 (완료율 비교, 분담 비율, 위클리 리포트)

## 플랫폼 특수 기능
- iOS Dynamic Island: 집안일 진행 타이머
- Android Samsung Now Bar + Glance Widget
- WearOS/watchOS: 완료 체크 탭

## 다음 단계 (Work Plan #93)
- ViewModel 바인딩 (`koinViewModel`)
- UseCase 레이어 추가 (`HomeTasksDomain`)
- Firebase Auth + Firestore 실제 연결
- TeamSetupScreen / TeamDashboard 실동작

## 모듈 재사용
| 모듈 | 재사용률 |
|------|---------|
| CoupleSync 팀 공유 | 80% |
| HabitFlow 스트릭/뱃지 | 75% |
| moon-sync/auth/analytics/billing/ui-kmp | 100% |

---

## ADR-001: 팀 공유 모델
- **Context**: 2인 이상 가구에서 집안일 배정 + 완료 확인 구조 필요
- **Options**: (a) CoupleSync 팀 모델 재사용, (b) 자체 팀 모델, (c) Firestore 단순 공유 문서
- **Decision**: CoupleSync 팀 공유 구조 80% 재사용 + 포인트 기반 완료 확인
- **Rationale**: 검증된 실시간 동기화 로직, 멤버 초대 링크 재활용, 개발 속도 극대화
- **Impact**: medium
- **확장**: 체크 vs 포인트 듀얼 모드 (Free 체크만, Premium 포인트+뱃지)
