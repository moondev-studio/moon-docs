# MoodFlow — Product Overview
> Version: 1.0.0 | Updated: 2026-04-08 | Status: 개발 진행중

## 제품 개요
| 항목 | 내용 |
|------|------|
| 앱명 | MoodFlow |
| 슬로건 | 감정의 흐름을 기록하라 |
| 타겟 | 정신건강/자기성찰 관심자 |
| 플랫폼 | Android, iOS |
| 번들 ID | com.moondeveloper.moodflow |

## 핵심 기능
- 일일 감정 기록 (이모지 + 메모)
- 감정 패턴 분석 (주/월간 차트)
- 트리거 태그 (수면/운동/식사 연관)
- 알림 리마인더

## 아키텍처
- L1: moon-kmp-libs OSS
- L2: moon-app-libs (includeBuild)
- L3: moodflow/libs
- L4: moodflow/composeApp
- Backend: moon-server /api/moodflow/v1/* (예정)

## 현재 상태 (2026-04-08)
- 개발 진행 중
- MCP Product-Spec 참조
