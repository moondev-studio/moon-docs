# MoodFlow — Product Overview
> Version: 1.0.0 | Updated: 2026-04-08 | Status: 개발 진행중
> Work Plan: [68] / [77] | Ref: MoodFlow-Product-Spec (MCP)

## 제품 개요
| 항목 | 내용 |
|------|------|
| 슬로건 | "하루 5분, AI가 읽어주는 나의 감정 패턴" |
| 타겟 | 전세계 자기계발/멘탈 관리 관심 20~40대 |
| 플랫폼 | Android, iOS, Web, WearOS, watchOS |
| 번들 ID | com.moondeveloper.moodflow |
| 수익 | AdMob (Free) + 구독 ₩4,900/월 / ₩39,900/년 |

## 현재 구현 완료 (Work Plan #77)
- 감정 선택 UI (이모지 + 강도 슬라이더) — `RecordScreen.kt`
- 메모 입력
- Room KMP 로컬 저장
- Firestore 동기화 (moon-sync-kmp)
- Navigation: `navigation/Screen.kt`

### 코드 상태 (실측)
- `composeApp/src/commonMain/kotlin/com/moondeveloper/moodflow/ui/record/RecordScreen.kt`
- `composeApp/src/commonMain/kotlin/com/moondeveloper/moodflow/navigation/Screen.kt`
- UseCase 미구현 (다음 단계에서 Domain 레이어 추가 필요)

## 핵심 기능
1. 감정 기록 (8개 이모지 + 강도 1~5, 메모 500자, Free 1회/일)
2. AI 인사이트 (Claude API 서버 프록시, 주간 리포트)
3. 통계 & 시각화 (히트맵/파이/트렌드)
4. 루틴 & 리마인더 (HabitFlow 스트릭 재사용)
5. 익스포트 (PDF/CSV — Premium)

## 플랫폼 특수 기능
- iOS Dynamic Island: 스트릭 D일 연속
- Android Samsung Now Bar: 스트릭 알림
- WearOS/watchOS: 빠른 감정 탭

## 다음 단계
- Domain/UseCase 레이어 추가 (`MoodDomain` 모듈)
- AI 인사이트 서버 프록시 (`/api/moodflow/v1/insights`)
- 주간 감정 분석 리포트 UI
- WearOS 빠른 기록

## 모듈 재사용
| 모듈 | 재사용률 |
|------|---------|
| HabitFlow 스트릭/알림 | 80% |
| moon-analytics/auth/sync/billing/i18n/ui-kmp | 100% |

---

## ADR-001: AI 인사이트 엔진
- **Context**: 감정 패턴 분석을 위한 LLM 선택 필요. API key 노출 방지 필수.
- **Options**: (a) Claude API 서버 프록시, (b) 온디바이스 LLM, (c) OpenAI 직접 호출
- **Decision**: Claude API → moon-server 프록시 (`/api/moodflow/v1/insights`)
- **Rationale**: API key 보안, 토큰 사용량 서버 제어, Claude의 감정 분석 품질 우수
- **Impact**: medium
- **가격 정책**: Free 월 1회 / Premium 무제한
