# FocusOn — Product Overview
> Version: 1.0.0 | Updated: 2026-04-08 | Status: Store 제출 준비

## 제품 개요
| 항목 | 내용 |
|------|------|
| 앱명 | FocusOn |
| 슬로건 | 집중의 흐름을 만들어라 |
| 타겟 | 학생, 재택근무자, 생산성 관심자 |
| 플랫폼 | Android, iOS |
| 번들 ID | com.moondeveloper.focuson |

## 핵심 기능
- 포모도로 타이머 (25분 집중 + 5분 휴식)
- 스터디룸 (다인 집중 공간, WebSocket)
- 세션 통계 (일/주/월 집중 시간)
- 광고 정책: 타이머 활성 중 배너 절대 미노출

## 핵심 기술 결정
- 타이머 활성 중 광고 차단: AdTimingPolicy (CRITICAL)
  - 포모도로 세션 도중 배너/인터스티셜 노출 금지
  - 세션 완료 시에만 보상형 광고 제공

## 현재 상태 (2026-04-08)
- AAB 33MB ✅
- AdMob v2 완료 (timer-safe ad logic)
- CRITICAL 테스트 통과 (AdTimingPolicyTest)
- Manual 잔여: 실기기 타이머 배너 검증, Play Console AAB
