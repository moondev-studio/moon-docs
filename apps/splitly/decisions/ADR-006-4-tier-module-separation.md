# ADR-006: 4-Tier Architecture Module Separation
- 버전: v1.0.0
- 날짜: 2026-03 (초기 설계)
- 상태: 결정됨 (소급 기록)

## 배경
4개 앱을 1인 개발하면서 코드 재사용률을 극대화해야 했다.
각 레이어의 공개 범위(OSS vs Private)와 의존성 방향을 명확히 분리할 필요.

## 결정
- **L1 — moon-kmp-libs (OSS, Maven Central)**
  - 12개 모듈: analytics, sync, ui, auth, billing, i18n, ocr, health, liveactivity, maps, wearable + build-logic
  - 앱 의존성 zero (Firebase, Play Services 직접 참조 금지)
  - `io.github.moondev-studio:*:1.0.0`으로 외부 배포
- **L2 — moon-app-libs (Private, includeBuild)**
  - 4개 모듈: moon-server-kmp, moon-firebase-auth-kmp, moon-firestore-sync-kmp, moon-firebase-billing-kmp
  - Firebase/서버 의존성 허용, `dependencySubstitution`으로 소스 레벨 연결
- **L3 — {app}/libs (앱별 비즈니스 로직)**
  - Splitly 기준 8개: splitly-analytics, splitly-sync, splitly-ui, splitly-auth, splitly-billing, splitly-i18n, splitly-settlement, splitly-ledger
  - 앱 도메인 모델과 UseCase 포함
- **L4 — {app}/composeApp (UI Layer)**
  - Compose Multiplatform UI, ViewModel, DI 설정
  - L1~L3를 조합하여 최종 앱 구성
- **Convention Plugin**: `build-logic/`으로 빌드 설정 통일 (L1, L2 각각 보유)

## 영향 범위
- `splitly/settings.gradle.kts` — 전체 모듈 구조 정의
- `moon-kmp-libs/settings.gradle.kts` — L1 모듈 12개
- `moon-app-libs/settings.gradle.kts` — L2 모듈 4개
- BetOnMe 등 향후 앱에서 L1/L2 재사용 (65% 재사용률 목표)

## 이전 니즈 참조
ADR-001 (Initial Design) — 아키텍처 상세 구현
