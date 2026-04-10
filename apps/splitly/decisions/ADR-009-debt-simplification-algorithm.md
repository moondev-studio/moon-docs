# ADR-009: Settlement Algorithm — Debt Simplification
- 버전: v1.0.0
- 날짜: 2026-03 (초기 설계)
- 상태: 결정됨 (소급 기록)

## 배경
다대다 더치페이에서 개별 채무(N명 x N명)를 최소 거래 수로 줄여야 했다.
NP-hard인 최적해 대신 실용적인 Greedy 알고리즘을 선택해야 했다.

## 결정
- **Greedy Balance-Based Algorithm** (`DebtSimplificationUseCase`)
  1. 모든 채무를 순잔액(net balance)으로 집계: 양수=받을 사람, 음수=줄 사람
  2. 받을 사람/줄 사람을 금액 내림차순 정렬
  3. 두 포인터로 Greedy 매칭: `min(receiverBalance, payerBalance)` 이체
  4. 한쪽 잔액이 0이면 다음으로 이동
- **시간 복잡도**: O(N log N) (정렬) + O(N) (매칭) = O(N log N)
- **최적성**: 최소 거래 수 보장은 아니지만 (max N-1건), 실용적 수준에서 충분
- **금액 단위**: `Long` (원화 기준, 소수점 없음)
- **통화 지원**: 기본값 KRW, `CrossGroupBalanceUseCase`로 그룹 간 잔액도 처리
- **모듈 위치**: `libs/splitly-settlement` (L3) — 순수 Kotlin, 외부 의존성 없음
- **테스트**: `DebtSimplificationUseCaseTest` + `DebtSimplificationBoundaryTest` (경계값)

## 영향 범위
- `libs/splitly-settlement/src/commonMain/.../usecase/debt/DebtSimplificationUseCase.kt` — 핵심 알고리즘
- `libs/splitly-settlement/src/commonMain/.../usecase/debt/CrossGroupBalanceUseCase.kt` — 그룹 간 잔액
- `libs/splitly-settlement/src/commonMain/.../model/` — `Debt`, `OptimizedPayment`, `PendingSettlementSummary`
- `composeApp/.../presentation/optimization/DebtOptimizationViewModel.kt` — UI 연결
- `libs/splitly-settlement/src/commonTest/` — 단위 테스트 + 경계값 테스트

## 이전 니즈 참조
ADR-006 (4-Tier Module Separation) — L3 앱 라이브러리에 도메인 로직 배치
