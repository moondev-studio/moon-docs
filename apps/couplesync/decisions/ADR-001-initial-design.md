# ADR-001: Initial Design
- 버전: v1.0.0
- 날짜: 2026-03 ~ 2026-04
- 상태: 결정됨

## 배경
MoonDeveloper 4개 앱 동시 개발 시작.
KMP(Kotlin Multiplatform) 기반 코드 최대 재사용 전략.

## 결정
- 아키텍처: 4-Tier KMP 모듈 시스템
  - L1: moon-kmp-libs (OSS, Maven Central)
  - L2: moon-app-libs (Private, includeBuild)
  - L3: {app}/libs (앱별 비즈니스 로직)
  - L4: {app}/composeApp (UI)
- 상태관리: MVI 통일
- DI: Koin
- 네트워킹: Ktor 3.x
- 백엔드: moon-server (Spring Boot, Railway)

## 영향 범위
- 전체 앱 공통 적용
- moon-kmp-libs: io.github.moondev-studio:*:1.0.0

## 이전 니즈 참조
최초 설계 — 이전 ADR 없음
