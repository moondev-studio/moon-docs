# ADR-003: iOS 심사 거절 수정 — v1.0.5
- 버전: v1.0.5
- 날짜: 2026-04-08
- 상태: 결정됨

## 배경
Apple 심사에서 4개 이슈로 거절:
1. PassKit framework 포함 (guideline 2.1 — 미사용 API)
2. ATT permission iOS 26.x 미대응
3. 계정 삭제 기능 미구현
4. IAP 복원 기능 미구현

## 결정
1. PassKit → build.gradle에서 제거
2. ATT → iOS 26 이상 조건부 처리
3. 계정 삭제 → AccountDeletionScreen 신규 구현
4. IAP 복원 → RestorePurchaseButton 추가

## 영향 범위
- Work Plan #90
- composeApp/src/iosMain/

## 이전 니즈 참조
ADR-002: IAP 구독 모델 (복원 기능은 IAP 모델 결정 시 함께 설계했어야 함 — 교훈)
