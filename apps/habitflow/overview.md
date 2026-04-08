# HabitFlow — Product Overview
> Version: 1.0.0 | Updated: 2026-04-08 | Status: Store 제출 준비

## 제품 개요
| 항목 | 내용 |
|------|------|
| 앱명 | HabitFlow |
| 슬로건 | 습관을 흐르게 하라 |
| 타겟 | 자기계발 관심 20~40대 |
| 플랫폼 | Android, iOS |
| 번들 ID | com.moondeveloper.habitflow |

## 핵심 기능
- 습관 생성/편집 (목표, 빈도, 알림)
- 스트릭 트래킹 (연속 달성 기록)
- 진행률 시각화 (히트맵, 그래프)
- 동기부여 알림 (Firebase FCM)
- 뱃지 시스템

## 수익 모델
| Tier | 가격 | 기능 |
|------|------|------|
| Free | 무료 | 습관 3개, 광고 있음 |
| Pro Monthly | ₩3,900/월 | 무제한 습관, 고급 통계, 광고 없음 |
| Pro Annual | ₩29,900/년 | Pro + 2개월 무료 |

## 아키텍처
- L1: moon-kmp-libs OSS
- L2: moon-app-libs (includeBuild)
- L3: habitflow/libs
- L4: habitflow/composeApp
- Backend: moon-server /api/habitflow/v1/*

## 현재 상태 (2026-04-08)
- AAB 25MB ✅
- AdMob v2 완료 (real IDs)
- OSS 5-module switch 완료
- Manual 잔여: Play Console AAB 업로드, Xcode Archive → ASC
