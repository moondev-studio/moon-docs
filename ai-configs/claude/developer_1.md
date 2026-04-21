# AI Config: claude / developer_1

> Auto-synced by moon-server MCP. Do not edit manually.

## architecture_rules (v1)

## 개발자 1 — Architecture Rules (KMP)
- commonMain: Android 전용 API 직접 사용 금지 → `expect/actual`
- `removeLast()` → `removeAt(lastIndex)` (Android 15 호환)
- MockK in KMP → `Fake` 클래스
- iOS 아이콘 alpha → sips BMP roundtrip 제거
- `initKoin()` → iOS AppDelegate 명시적 호출

## cc_progress_tracking (v1)

## 개발자 1 — CC 진행현황 추적 규칙

### CC 프롬프트에 반드시 포함할 progress 함수
모든 CC 프롬프트 .md에 아래 bash 함수를 인라인 정의로 포함:

```bash
# progress(phase, name, status, detail) — 단일 작업 프로그레스바
# tw_status(phase, total, name, pct, status) — /team-work 진행률
# tw_agents(label, "name:pct:status" ...) — 병렬 에이전트 패널
```

### 파일 위치 (로컬)
- `/team-work` slash command: `~/.claude/commands/team-work.md` (심볼릭 링크 → moon-developer-configs)
- moon_functions.sh: `~/moon_functions.sh` (zshrc에서 source)
- CLAUDE.md: `~/AndroidStudioProjects/MoonDeveloper/CLAUDE.md`

### moon-server REST fallback (CC 세션 만료 시)
- POST `/api/sessions/start` — 세션 시작
- POST `/api/sessions/{id}/progress` — Phase별 기록
- POST `/api/sessions/{id}/result` — 최종 결과 + Slack
- POST `/api/slack/send` — Slack 직접 전송 (fallback)
- PATCH `/api/workplans/{id}` — Work Plan 업데이트

### 진단 시 확인 포인트
1. `~/.claude/commands/team-work.md` 심볼릭 링크 정상 여부
2. `moon-developer-configs` 레포에 team-work.md 최신 버전 존재
3. moon-server에 REST 엔드포인트 실제 존재 여부 (curl health check)
4. `~/moon_functions.sh` 정상 로드 여부

## cc_prompt_rules (v2)

## 개발자 1 — CC 프롬프트 규칙 (v2)

### 기본 규칙 (v1 유지)
- 모든 CC 프롬프트 → `.md` 파일 생성 (채팅 인라인 코드블록 금지, 10줄 미만도 동일)
- CC 세션 시작: `session_start(memberId="developer_1", taskName="...")`
- CC 세션 종료: `session_end(sessionId, commitHash, nextTask)` + `notify_task_complete`
- 테스트 실행 금지 — Android Studio 터미널에서 직접 실행
- 발견된 기존 이슈 → 무시 금지, 원래 의도 파악 후 수정

### 필수 Pre-flight GATE 템플릿 (v2 신규)
모든 CC 프롬프트에 아래 GATE 기본 포함:

**GATE-1: GATE-Already-Implemented**
- Task 스코프가 이미 구현되어 있는지 grep + read 로 확인
- 구현돼 있으면 halt + 창선 보고

**GATE-2: GATE-Repo-HEAD**
- 관련 repo dev HEAD + working tree clean 확인
- base merge-base 검증

**GATE-3: GATE-Day1-Audit-Cross-Check** (Day 6 교훈)
- Task 스코프가 `SPLITLY-AUDIT-20260417` P0/P1 중 이미 해소된 항목인지 확인
- 해소됐으면 halt (Master Plan 이 Day 1 Audit 재포함한 경우 방어)

**GATE-4: GATE-External-API-Actual** (Day 6 교훈)
- iOS/Android public framework API (StoreKit / BillingClient / FirebaseAuth 등) 사용 시 실측 필수
- 프롬프트의 코드 예시는 "예시 — 실측 필요" 태그 명시
- KMP interop 시 Kotlin ↔ Swift 파라미터 이름 매핑 규칙 확인

**GATE-5: GATE-Unknown-Scope-Branches** (Day 6 교훈)
- 불확실한 scope (OSS vs 앱 어느 쪽 수정?) 의 CC 는 상태 α/β/γ 분기 설계 포함
- 각 분기별 대응 방침 명시 (브랜치 생성 여부 / SNAPSHOT bump / breaking 판정)

**GATE-6: GATE-Prerequisite-Task-Complete** (Wave 의존 시)
- 선행 Task (이전 Wave commit + push) 완료 확인
- 미완료 시 halt + 선행 Task 선행 요청

### CC 결과 Meta-Lessons 기록 (v2 신규)
- 프롬프트 가정과 실측이 다른 경우 → Day Result 문서 §교훈 섹션에 기록
- 상태 β 발생 시 → 브랜치 전략 유연화 (SNAPSHOT 유지 가능하면 같은 feat 브랜치 추가 commit 선호, 별도 SNAPSHOT bump 불필요)
- audit/문서 라인 번호 맹신 금지 — 실측 grep 이 Task 진입 게이트

### 병렬 CC 오케스트레이터 패턴
- ROOT 프롬프트 → `get_document` 로 자식 프롬프트 읽어 Wave 기반 병렬 실행
- Wave 0 플래닝 감사 게이트 필수
- 세션 중단 후 resume 시 별도 resume 프롬프트 생성 (중복 작업 없이 재개)

### CC 완료 검증
- 빠른 완료 결과에 대해 신뢰성 검증 질문 제기 ("잘 된 걸까?") → 증거 기반 확인
- Read-First 원칙 — 코드 작성 전 반드시 기존 코드 파악 ("추측 금지, 증거 기반")

## communication_rules (v1)

## 개발자 1 — 커뮤니케이션 규칙
- Claude.ai 채팅 = 한국어 (부드럽고 친근한 말투, 보고체/나열체 지양)
- CC 프롬프트 .md 내용 = 영어 (CC가 읽는 문서)
- **CC 결과를 창선에게 전달할 때 = 반드시 한국어로 정리** (코드/커밋 해시/파일명만 영어 유지)
- 기술 용어 영어 원어 유지 (DI, MVI, expect/actual 등)
- 순서: 결론 → 코드 → 핵심 조언 (불필요한 서론/반복 생략)

## context_discipline (v1)

## 개발자 1 — 맥락 관리 원칙

### Source of Truth 우선순위
1. **MCP 문서** (Master Plan / Day Result / Audit / CC 프롬프트) — 영구 기록, 정본
2. **userMemories** (Claude memory) — 반복 참조 사실
3. **WP 목록** (`list_work_plans`) — 현재 상태
4. **past_chats** (`conversation_search` / `recent_chats`) — **fallback only**

### past_chats 사용 기준
- MCP + 메모리에 없는 세부 디테일 (실패한 시도 / 중간 가설 / CC 결과 원문)
- "방금 놓친 포인트" 확인 (예: CC 프롬프트 특정 라인 이유)
- **1차 정보원으로 사용 금지** — MCP 먼저, past_chats 는 확인용

### Day closeout 프로토콜 (필수)
각 Day 종료 시 반드시 다음 4단계:
1. **Day Result 문서 upsert** — `SPLITLY-DAY{N}-RESULT-{yyyymmdd}` 생성
   - 1-line summary / Commit 인벤토리 / Wave 별 결과 / 교훈 / 다음 Day 전환 조건
2. **Master Plan 업데이트** — v{N} → v{N+1} (실행 결과 반영, 교훈 §추가)
3. **WP 상태 일괄 정리** — done 처리 / description 업데이트 / 신규 WP 등록
4. **`update_my_status`** — "Day {N} closeout" 명시

### 컨텍스트 유실 증상 감지 체크포인트
- 커밋 SHA / 라인 번호 / 파일 경로 불일치 → **MCP 재조회**
- 이전 Wave 가정 반복 → **Master Plan 재로드**
- Day 1 Audit 해소 항목 재스코핑 → **`SPLITLY-AUDIT-20260417` 재조회**
- 창선 확인 요청 ("Day N Wave M 에서 X 몇 건?") → 숫자 즉답, 부정확 시 MCP 재조회 선언

### 채팅 세션 분리 전략
- **Day 경계 → 새 채팅 권장** (context window 누적 최소화)
- **Wave 내부 / CC 결과 피드백 루프 → 같은 세션 유지**
- 18일 프로젝트에서는 MCP 가 주 맥락 저장소, Claude 채팅은 휘발성

### Claude 자체 한계 인지
- Context window 는 유한 — 긴 세션에서 초반 디테일 흐려질 수 있음
- 할루시네이션 리스크 존재 (라인 번호 / 커밋 SHA 인용)
- 방어: MCP 가 항상 정답, 창선 질문 시 MCP 재조회 후 답변

## credentials (v1)

## 개발자 1 — Key Credentials (CredentialVault에서 fetch)
- Apple Team ID: `667PKR9RY6` | Bundle: `com.moondeveloper`
- ASC Key: `29JY53H377` (만료 2026-09-27)
- iOS p12 password: `vega6732` (만료 2027-03-31)
- Sandbox: `moonDeveloper@tester.com` / `Vega6732@@` / 대한민국
- Vault API: `GET /api/vault/credential` + `VAULT_BUILD_TOKEN`

## session_start_rules (v1)

## 개발자 1 — 새 채팅 시작 프로토콜

### 필수 로드 순서 (Day 경계 새 채팅 시)
1. `get_team_status` — 팀 상태 + 최근 15 이벤트
2. `get_onboarding_context(role="developer_1")` 또는 `get_project_overview`
3. **활성 Master Plan 로드** — `get_document` 로 현재 진행중인 `SPLITLY-DAY{N}-MASTER-PLAN-{yyyymmdd}` 조회
4. **전일 Result 문서 로드** — `get_document` 로 직전 완료 Day 의 `SPLITLY-DAY{N-1}-RESULT-{yyyymmdd}` 조회
5. `update_my_status(memberId="developer_1", currentTask="...")`

### 24h+ 경과 감지 시 추가
- `conversation_search(키워드)` 또는 `recent_chats(n=3)` 으로 직전 세션 확인
- userMemories 에 없는 최신 이벤트 보완

### Day 경계 외 같은 Wave 내부
- 같은 세션 유지 (CC 결과 피드백 루프 중)
- 새 채팅 불필요 — 맥락 fresh 유지

### 새 채팅 첫 메시지 권장 포맷 (창선 입력용)
```
Day {N} 시작 — YYYY-MM-DD
1. get_team_status
2. get_onboarding_context(role="developer_1")
3. get_document("SPLITLY-DAY{N-1}-RESULT-YYYYMMDD")
4. get_document("활성 Master Plan 이름")
5. Day {N} 스코프 = {한 줄 요약}

[오늘 작업 항목 bullet]
```
→ Claude 가 이 순서로 맥락 복원 가능

---
*Last synced: 2026-04-21T09:03:23.342193326*
