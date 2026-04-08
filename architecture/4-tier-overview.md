# 4-Tier KMP 아키텍처

## 구조

```
L1: moon-kmp-libs (OSS, Maven Central)
    io.github.moondev-studio:moon-ui-kmp:1.0.0
    io.github.moondev-studio:moon-billing-kmp:1.0.0
    io.github.moondev-studio:moon-analytics-kmp:1.0.0
    io.github.moondev-studio:moon-sync-kmp:1.0.0
    io.github.moondev-studio:moon-i18n-kmp:1.0.0
    io.github.moondev-studio:moon-auth-kmp:1.0.0
    io.github.moondev-studio:moon-ocr-kmp:1.0.0

L2: moon-app-libs (Private, includeBuild)
    moon-server-kmp          — API 클라이언트
    moon-firebase-auth-kmp   — Firebase Auth KMP 래퍼
    moon-firestore-sync-kmp  — Firestore 동기화

L3: {app}/libs (앱별 비즈니스 로직)
    {app}-core / {app}-data / {app}-domain / {app}-ui

L4: {app}/composeApp (UI 진입점)
    Android + iOS 공통 Compose UI
```

## 핵심 원칙

- `commonMain`: Android 전용 API 직접 사용 금지 → `expect/actual` 패턴
- `removeLast()` → `removeAt(lastIndex)` (Android 15 API 충돌 방지)
- MockK KMP 미지원 → `Fake` 클래스 사용
- Koin `initKoin()` → iOS `AppDelegate`에서 명시적 호출 필수

## 앱별 코드 재사용률

| 앱 | Splitly 재사용 | 특이사항 |
|----|---------------|---------|
| CoupleSync | ~70% | 지출 분할 로직 공유 |
| HabitFlow | L1 모듈만 | 독립 비즈니스 로직 |
| FocusOn | L1 모듈만 | 타이머 중 광고 제어 필수 |

## Base Path

```
/Users/mcs/AndroidStudioProjects/MoonDeveloper/
├── moon-kmp-libs/    # L1 OSS
├── moon-app-libs/    # L2 Private
├── splitly/
├── habitflow/
├── couplesync/
├── focuson/
├── moon-server/
└── moon-docs/        # 이 레포
```
