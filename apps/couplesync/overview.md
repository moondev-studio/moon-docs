# CoupleSync — Product Overview
> Version: 1.0.0 | Updated: 2026-04-08 | Status: Store 제출 준비

## 제품 개요
| 항목 | 내용 |
|------|------|
| 앱명 | CoupleSync |
| 슬로건 | 함께 쓰고, 함께 정산 |
| 타겟 | 커플, 동거 파트너, 룸메이트 |
| 플랫폼 | Android, iOS |
| 번들 ID | com.moondeveloper.couplesync |

## 핵심 기능
- 공동 지출 기록 (Splitly 코어 ~70% 재사용)
- 커플/팀 가계부 (공유 장부)
- 정산 제안 자동 생성
- 카테고리별 지출 분석

## 아키텍처
- Splitly 코드 약 70% 재사용
- OSS dual-substitution 유지 (includeBuild 제거 시 R8 duplicate class 에러)
- Firebase projectId: couplesync-73b3e
- Backend: moon-server /api/couplesync/v1/*

## 현재 상태 (2026-04-08)
- AAB 17.6MB ✅
- AdMob v2 완료 (6 real IDs)
- Firebase Blaze 업그레이드 ✅
- Manual 잔여: Play Console AAB, Xcode → ASC
