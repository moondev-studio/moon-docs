# ADR-005: moon-kmp-libs OSS Maven Central Publication
- 버전: v1.0.3
- 날짜: 2026-03 (commit 3d4759c 기반)
- 상태: 결정됨 (소급 기록)

## 배경
moon-kmp-libs는 4개 앱(Splitly, BetOnMe, ureen 등)에서 공유하는 KMP 공통 라이브러리.
초기에는 `includeBuild`로 로컬 참조했으나, 앱별 독립 빌드와 CI 안정성을 위해 Maven Central 배포 결정.

## 결정
- **Group ID**: `io.github.moondev-studio` (Sonatype OSSRH Central Portal)
- **배포 대상 7개 모듈**: moon-analytics-kmp, moon-sync-kmp, moon-ui-kmp, moon-auth-kmp, moon-billing-kmp, moon-i18n-kmp, moon-ocr-kmp
- **추가 모듈** (미배포): moon-health-kmp, moon-liveactivity-kmp, moon-maps-kmp, moon-wearable-kmp
- **버전**: 1.0.0 (version catalog `moonKmpLibs = "1.0.0"`)
- **빌드 도구**: vanniktech 0.30.0 (`publishToMavenLocal` 검증 후 Central 배포)
- **앱 의존성**: `libs.versions.toml`에서 Maven Central artifact로 참조
- **L2 (moon-app-libs)**: Private으로 유지, `includeBuild` + `dependencySubstitution`으로 소스 참조
  - moon-server-kmp, moon-firebase-auth-kmp, moon-firestore-sync-kmp, moon-firebase-billing-kmp
- **OSS 규칙**: L1 모듈은 Firebase/Play 등 앱 의존성 zero 원칙 유지

## 영향 범위
- `moon-kmp-libs/` — 전체 (12개 모듈, 5개 타겟: Android, iOS, Desktop, JS, WASM)
- `splitly/gradle/libs.versions.toml` — `io.github.moondev-studio:*:1.0.0`
- `splitly/settings.gradle.kts` — L1은 Maven, L2는 `includeBuild` 유지
- 전체 앱 포트폴리오에 동일 적용

## 이전 니즈 참조
ADR-001 (Initial Design) — 4-Tier KMP 모듈 시스템 L1 구체화
