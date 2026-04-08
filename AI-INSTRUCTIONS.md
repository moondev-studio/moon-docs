# AI Instructions — MCP-First Context & Decision Auto-Record
> Target audience: 모든 채팅/CC 에이전트 (developer_1, planner, cc_agent 등)
> Last updated: 2026-04-08

## Context Retrieval Rules (MCP-First)

모든 채팅/CC 에이전트는 아래 우선순위로 컨텍스트를 조회해야 함:

### 새 채팅 시작 시 (필수 순서)
1. `get_team_status` → 팀 현황 + 최근 이벤트 파악
2. `get_project_overview` → 전체 Work Plan 파악
3. `search_documents(keyword)` → 관련 기획서/가이드 검색
4. `get_decisions()` → 최근 결정사항 확인
5. 위에서 찾을 수 없는 정보라면 →
   - "이전 채팅을 직접 참조할까요?" 또는
   - "관련 문서를 업로드해 주세요" 중 선택 제시

### 질문 처리 전 체크
- 앱 기능/방향 관련 질문 → `search_documents` 먼저
- 과거 결정 관련 질문 → `get_decisions` + `search_history`
- 현재 작업 상태 질문 → `get_project_overview`
- MCP에서 찾을 수 없을 때만 → 과거 채팅 참조 또는 사용자에게 물어봄

---

## Planning Decision Auto-Record Rules (기획 결정 자동 기록)

### 감지 키워드 (아래 패턴 발견 시 즉시 record_decision 실행)
- "~로 변경하기로 했어 / 하자"
- "~방향으로 가자 / 결정했어"
- "~를 추가하자 / 제거하자"
- 수익 모델 변경, 핵심 기능 추가/삭제
- 아키텍처 또는 기술 스택 변경

### 자동 실행 순서
1. `record_decision(appName, title, content, impact)` 즉시 실행
2. `upsert_document` → 관련 기획서 업데이트 알림
3. ADR 파일 생성 권고 (`moon-docs/{app}/decisions/ADR-{n}.md`)

### record_decision 필드 가이드
- `appName`: 해당 앱명 (splitly|habitflow|couplesync|focuson|global)
- `title`: 결정 요약 (20자 이내)
- `content`: 배경 + 결정 내용 + 영향 범위
- `impact`: high|medium|low

### ADR 작성 기준 (impact=high 시 필수)
```markdown
# ADR-{n}: {결정 제목}
- 버전: v{x.y.z}  날짜: YYYY-MM-DD  상태: 결정됨

## 배경 / 결정 / 영향 범위 / 이전 니즈 참조
```

---

## Planning Docs Location
- Git: `moondev-studio/moon-docs` (Private Repo)
- 폴더: `apps/{appName}/overview.md` (최신 1개 유지)
- ADR: `apps/{appName}/decisions/ADR-{n}-{slug}.md`
- MCP 인덱스: `{App}-Planning-v{version}` 문서
