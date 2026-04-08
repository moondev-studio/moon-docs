# Planning System Guide
> Version: 1.0.0 | 2026-04-08

## 채팅 → 기획서 자동 업데이트 워크플로

### AI 감지 규칙 (모든 채팅/CC에 적용)
기획 결정 발생 감지 키워드:
- "~로 변경하기로 했어", "~방향으로 가자", "~를 추가하자"
- 수익 모델 변경, 핵심 기능 추가/삭제, 아키텍처 변경

감지 시 자동 실행:
1. record_decision(MCP) 즉시 실행
2. 해당 ADR 파일 생성 요청
3. overview.md 업데이트 알림

### MCP 우선 참조 규칙
```
새 채팅 시작 시:
  1. get_project_overview + get_team_status
  2. search_documents(keyword)
  3. get_decisions()
  4. 없으면 → 사용자에게 선택지 제시
```

### ADR 템플릿
```markdown
# ADR-{번호}: {결정 제목}
- 버전: v{x.y.z}
- 날짜: YYYY-MM-DD
- 상태: 결정됨 | 검토중 | 폐기됨

## 배경
이전 버전에서의 상황

## 결정
구체적으로 무엇을 결정했는가

## 영향 범위
- 변경된 파일/섹션
- 관련 서버 API 변경
- 관련 Work Plan #번호

## 이전 니즈 참조
이전 ADR 번호 및 연관 결정
```
