# PawLife — Product Overview
> Version: 1.0.0 | Updated: 2026-04-08 | Status: 기획 확정 (Work Plan #92 대기)
> Work Plan: [67] / [92] | Ref: PawLife-Product-Spec (MCP)

## 제품 개요
| 항목 | 내용 |
|------|------|
| 슬로건 | "온 가족이 함께 키우는 반려동물 다이어리" |
| 타겟 | 전세계 반려동물 보호자, 특히 가족/커플 단위 |
| 플랫폼 | Android, iOS, Web, WearOS, watchOS |
| 번들 ID | com.moondeveloper.pawlife |
| 수익 | AdMob (Free) + 구독 ₩3,900/월 / ₩29,900/년 |

## 핵심 기능 (Work Plan #92)
1. 반려동물 프로필 CRUD (멀티펫, Free 1마리)
2. GPS 산책 트래킹 (핵심 차별점, 경로 표시, 거리/시간/칼로리/속도)
3. 건강 기록 카드 (예방접종/검진/투약, D-day 알림)
4. 일상 기록 (급여, 사진 타임라인, 메모)
5. 팀 공유 (가족 공동 관리) — CoupleSync 재사용
6. 리마인더 (산책/급여/예방접종/생일)

### 코드 상태 (실측)
- `composeApp/.../navigation/Screen.kt`
- `libs/pawlife-feature-walk/.../WalkScreen.kt`
- `libs/pawlife-feature-walk/.../WalkUseCase.kt` ← **4개 앱 중 유일하게 UseCase 구현 존재**
- Domain/Data 분리 선행 사례로 활용 가능

## 기술 결정
- GPS: `moon-maps-kmp` (신규 OSS, Google Maps SDK + MapKit 공통 Wrapper)
- 사진: Coil + Firebase Storage
- 산책 기록: 실시간 Firestore GeoPoint 배열
- 건강 데이터: `moon-health-kmp` (HealthKit + Health Connect)
- 타이머: FocusOn 재사용 (산책 시간 측정)

## 플랫폼 특수 기능
- iOS Dynamic Island / Live Activity: 산책 중 실시간 경과 시간 + 거리
- Android Samsung Now Bar + Glance Widget
- WearOS/watchOS: 산책 시작/종료, 심박수(HealthKit)

## 신규 OSS 모듈 의존
| 모듈 | 우선순위 | 용도 |
|------|---------|------|
| moon-maps-kmp | P1 | GPS/지도 Wrapper |
| moon-liveactivity-kmp | P1 | Dynamic Island / Now Bar |
| moon-wearable-kmp | P2 | WearOS/watchOS |
| moon-health-kmp | P2 | HealthKit/Health Connect |

## 모듈 재사용
| 모듈 | 재사용률 |
|------|---------|
| HabitFlow 스트릭 | 70% |
| FocusOn 타이머 | 80% |
| CoupleSync 팀 공유 | 75% |
| moon-sync/auth/analytics/billing/ui-kmp | 100% |

---

## ADR-001: GPS 전략
- **Context**: 산책 트래킹이 PawLife 핵심 차별점. Android/iOS 지도 SDK 상이.
- **Options**: (a) moon-maps-kmp 신규 OSS Wrapper, (b) Mapbox KMP, (c) 앱별 플랫폼 직접 구현
- **Decision**: Google Maps SDK (Android) + MapKit (iOS) KMP Wrapper → `moon-maps-kmp` 신규 OSS 모듈 분리
- **Rationale**: 플랫폼 네이티브 SDK 품질 유지, 라이선스 비용 없음, 타 신규 앱(HomeTasks 위치 확장 등) 재사용 가능, expect/actual 패턴으로 깔끔한 KMP 구조
- **Impact**: high (신규 OSS 모듈 개발 필요)
- **GPS Mock**: LocationProvider Fake로 자동화 테스트 커버리지 100%
