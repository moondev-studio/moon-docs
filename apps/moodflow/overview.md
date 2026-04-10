# MoodFlow — Product Overview
> Version: 1.0.0 | Updated: 2026-04-10 | Status: planned (design-first)
> Work Plan: [68] / [77] | Ref: MoodFlow-Product-Spec (MCP)

## 제품 개요
| 항목 | 내용 |
|------|------|
| 슬로건 | "하루 5분, AI가 읽어주는 나의 감정 패턴" |
| 타겟 | 전세계 자기계발/멘탈 관리 관심 20~40대 |
| 플랫폼 | Android, iOS, Web, WearOS, watchOS |
| 번들 ID | com.moondeveloper.moodflow |
| 수익 | AdMob (Free) + 구독 ₩4,900/월 / ₩39,900/년 |

## 앱 컨셉
MoodFlow는 일일 감정/기분 트래킹과 저널링을 결합한 앱이다.
사용자가 매일 자신의 감정을 이모지와 강도로 기록하고, 자유 형식 저널을 작성하면
AI가 감정 패턴을 분석하여 트리거를 식별하고 인사이트를 제공한다.

### 핵심 가치
1. **간편한 기록** — 이모지 탭 + 강도 슬라이더로 5초 무드 기록
2. **깊이 있는 저널링** — 감정에 연결된 자유 형식 일기
3. **트리거 분석** — 태그 기반 감정 유발 요인 추적
4. **AI 인사이트** — Claude API 기반 주간/월간 감정 패턴 리포트
5. **명상 가이드** — 현재 기분에 맞는 명상/호흡 추천

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

## MVP 범위 (v1.0.0)
- 감정 기록 (8개 이모지, 강도 1~5, 메모 500자)
- 트리거 태깅 (사전 정의 + 커스텀 태그)
- 기본 저널링 (감정 연동 텍스트 일기)
- 통계 대시보드 (히트맵, 파이 차트, 트렌드)
- 일일 리마인더 알림
- AI 인사이트 (주간 리포트, Free 월 1회 / Premium 무제한)
- 데이터 내보내기 (CSV/PDF — Premium)

## 수익 모델
| 티어 | 가격 | 제공 |
|------|------|------|
| Free | ₩0 | 일일 1회 기록, 기본 통계, 월 1회 AI 인사이트 |
| Premium Monthly | ₩4,900/월 | 무제한 기록, 고급 통계, 무제한 AI 인사이트, 내보내기 |
| Premium Annual | ₩39,900/년 | Premium Monthly 동일 (32% 할인) |

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

## 기술 스택
- 아키텍처: 4-Tier KMP (L1~L4)
- 상태 관리: MVI
- DI: Koin
- 네트워킹: Ktor 3.x
- 로컬 DB: Room KMP
- 동기화: Firestore (moon-sync-kmp)
- AI: Claude API via moon-server proxy

## 모듈 재사용
| 모듈 | 재사용률 |
|------|---------|
| HabitFlow 스트릭/알림 | 80% |
| moon-analytics/auth/sync/billing/i18n/ui-kmp | 100% |

## 다음 단계
- Domain/UseCase 레이어 설계
- AI 인사이트 서버 프록시 구현 (`/api/moodflow/v1/insights`)
- UI 와이어프레임 확정
- WearOS 빠른 기록 프로토타입

---

## ADR-001: AI 인사이트 엔진
- **Context**: 감정 패턴 분석을 위한 LLM 선택 필요. API key 노출 방지 필수.
- **Options**: (a) Claude API 서버 프록시, (b) 온디바이스 LLM, (c) OpenAI 직접 호출
- **Decision**: Claude API → moon-server 프록시 (`/api/moodflow/v1/insights`)
- **Rationale**: API key 보안, 토큰 사용량 서버 제어, Claude의 감정 분석 품질 우수
- **Impact**: medium
- **가격 정책**: Free 월 1회 / Premium 무제한
