# Apps — Global Development Goals
> Updated: 2026-04-08 | Ref: NewApps-Global-Dev-Goals (MCP Document)

## 공통 아키텍처 목표
- **OSS 우선**: 신규 앱은 Maven Central 버전만 사용 (includeBuild 금지)
- **KMP 4-Tier**: L1(moon-kmp-libs OSS) → L2(moon-app-libs) → L3({app}/libs) → L4({app}/composeApp)
- **Wearable**: WearOS + watchOS (`moon-wearable-kmp`)
- **Dynamic Island / Now Bar**: iOS Live Activity + Samsung Now Bar (`moon-liveactivity-kmp`)
- **건강 데이터**: Google Fit / Health Connect + HealthKit (`moon-health-kmp`)
- **지도/GPS**: Google Maps + MapKit (`moon-maps-kmp`)

## 플랫폼 지원 매트릭스
| 기능 | PawLife | MoodFlow | FreelancePay | HomeTasks |
|------|---------|----------|--------------|-----------|
| Android / iOS / Web | ✅ | ✅ | ✅ (Web 메인) | ✅ |
| WearOS / watchOS | ✅ | ✅ | ❌ | ✅ |
| Android / iOS Widget | ✅ | ✅ | ✅ | ✅ |
| Dynamic Island | ✅ 산책 | ✅ 스트릭 | ❌ | ✅ 집안일 |
| Samsung Now Bar | ✅ | ✅ | ❌ | ✅ |

## 신규 OSS 모듈
| 모듈 | 용도 | 우선순위 | 소비 앱 |
|------|------|---------|---------|
| moon-maps-kmp | GPS / 지도 Wrapper | P1 | PawLife |
| moon-liveactivity-kmp | Dynamic Island / Now Bar | P1 | PawLife, HomeTasks, MoodFlow |
| moon-wearable-kmp | WearOS / watchOS | P2 | PawLife, MoodFlow, HomeTasks |
| moon-health-kmp | HealthKit / Health Connect | P2 | PawLife |

## 코드 재사용 전략
| 소스 앱 | 재사용 대상 | 재사용률 |
|--------|-----------|---------|
| Splitly | CoupleSync (정산 코어) | 70% |
| Splitly | FreelancePay (금액 계산 UI) | 60% |
| FocusOn | FreelancePay (타이머 코어) | 85% |
| FocusOn | PawLife (산책 타이머) | 80% |
| HabitFlow | MoodFlow (스트릭/뱃지) | 80% |
| HabitFlow | HomeTasks (스트릭/보상) | 75% |
| HabitFlow | PawLife (루틴) | 70% |
| CoupleSync | HomeTasks (팀 공유) | 80% |
| CoupleSync | PawLife (가족 공유) | 75% |

## 출시 우선순위
1. **Stage 1 (스토어 제출중)**: Splitly, HabitFlow, CoupleSync, FocusOn
2. **Stage 2 (개발중)**: MoodFlow (#68/#77), HomeTasks (#70/#78/#93), FreelancePay (#69/#79)
3. **Stage 3 (기획 확정)**: PawLife (#67/#92)

## 글로벌 i18n
- 우선 언어: EN (기본) → KO → JA → ZH → ES
- `moon-i18n-kmp` 공통 사용, 스토어 메타데이터 동일 5개 언어
- 가격: USD 기준 → 스토어 자동 PPP 현지화

## QA 공통 전략
- Unit: KMP commonTest + Fake (MockK 금지) — Domain 90%+
- UI: Compose Test (Android) + XCUITest (iOS) — 70%+
- Integration: Ktor MockEngine + Firebase Emulator — 80%+
- E2E: Maestro (Android) + XCUITest (iOS) — 핵심 플로우 100%
- CI: GitHub Actions (모든 PR 자동 실행)

## Backend 엔드포인트
```
/api/pawlife/v1/*       → pets, walks, health, feeding, team, reminders
/api/moodflow/v1/*      → entries, insights (Claude proxy), reports
/api/freelancepay/v1/*  → projects, time-entries, invoices, reports
/api/hometasks/v1/*     → teams, tasks, schedules, stats
```

## 앱별 상세 문서
- [MoodFlow](./moodflow/overview.md)
- [HomeTasks](./hometasks/overview.md)
- [FreelancePay](./freelancepay/overview.md)
- [PawLife](./pawlife/overview.md)
