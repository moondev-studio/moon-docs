# ADR-002: MVP Scope
- Version: v1.0.0
- Date: 2026-04-10
- Status: proposed

## Context
MoodFlow 앱의 MVP 범위를 결정해야 한다.
감정 트래킹, 저널링, AI 인사이트, 명상 가이드 등 다양한 기능이 구상되어 있으나,
초기 출시를 위해 핵심 기능을 선별하고 단계적으로 확장해야 한다.

## Decision Drivers
- 빠른 시장 검증 (2~3개월 내 MVP 출시)
- 핵심 가치 제공: 간편한 감정 기록 + AI 인사이트
- KMP 모듈 재사용으로 개발 속도 극대화
- Free/Premium 수익 모델 초기 검증

## Considered Options

### Option A: Full Feature Launch
모든 기능 (감정 기록 + 저널링 + 통계 + AI + 명상 + WearOS) 동시 출시.
- 장점: 완성도 높은 첫 인상
- 단점: 개발 6개월 이상, 시장 검증 지연

### Option B: Core + AI MVP (선택)
감정 기록 + 트리거 태깅 + 기본 통계 + AI 인사이트만 MVP 출시.
저널링/명상/WearOS는 v1.1 이후.
- 장점: 2~3개월 출시, 핵심 차별점(AI) 조기 검증
- 단점: 초기 콘텐츠 부족 가능성

### Option C: Record-Only MVP
감정 기록 + 통계만 출시, AI/저널링 모두 후순위.
- 장점: 최소 1~2개월 출시
- 단점: 경쟁 앱 대비 차별점 부족

## Decision
**Option B: Core + AI MVP**를 선택한다.

### MVP v1.0.0 포함 기능 (Phase 1)
| 영역 | 기능 | Feature ID |
|------|------|------------|
| Mood | 일일 무드 기록 (이모지 + 강도 + 메모) | F-001, F-002, F-003 |
| Trigger | 트리거 태깅 (사전 정의 + 커스텀) | F-008, F-009 |
| Statistics | 히트맵, 파이 차트, 트렌드 | F-011, F-012, F-013, F-014 |
| Insight | AI 주간 리포트 | F-015 |
| Notification | 일일 리마인더 | F-018 |
| Streak | 연속 기록 카운터 | F-030 |
| Auth | 로그인/동기화 | F-026, F-027 |
| Paywall | Free/Premium 구분 | F-028, F-029 |

### Phase 2 (v1.1.0)
| 영역 | 기능 | Feature ID |
|------|------|------------|
| Journal | 저널 작성/검색 | F-005, F-006, F-007 |
| Mood | 다중 감정 선택 | F-004 |
| Trigger | 트리거-감정 상관관계 | F-010 |
| Insight | 월간 패턴/트리거 인사이트 | F-016, F-017 |
| Export | CSV/PDF 내보내기 | F-024, F-025 |

### Phase 3 (v1.2.0)
| 영역 | 기능 | Feature ID |
|------|------|------------|
| Meditation | 명상 추천/호흡/타이머 | F-021, F-022, F-023 |
| Streak | 배지/보상 | F-031 |
| WearOS | 빠른 감정 탭 | F-035 |

## Consequences

### Positive
- 2~3개월 내 MVP 출시 가능
- AI 인사이트로 경쟁 앱 대비 차별화
- 사용자 피드백 기반 Phase 2/3 우선순위 조정 가능
- HabitFlow 스트릭/알림 모듈 80% 재사용

### Negative
- 저널링 미포함으로 초기 사용자 체류 시간 제한 가능
- 명상 기능 없이 "웰니스 앱" 포지셔닝 약화

### Risks
- AI 인사이트 품질이 사용자 기대에 미치지 못할 경우 → 프롬프트 튜닝 + A/B 테스트
- Free 월 1회 AI 제한이 전환율에 미치는 영향 불확실 → 초기 3개월 무료 제공 후 분석

## References
- ADR-001: Initial Design (4-Tier KMP Architecture)
- MoodFlow Product Overview (`apps/moodflow/overview.md`)
- MoodFlow Feature Spec (`apps/moodflow/app/features.md`)
