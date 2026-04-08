# MCP 도구 레퍼런스

**연결**: `https://moon-server-production.up.railway.app/mcp`
**인증**: Bearer Token (VAULT_BUILD_TOKEN)

## 상태 조회

| 도구 | 파라미터 | 설명 |
|------|---------|------|
| `get_project_health` | - | 서버·DB·테이블 전체 상태 |
| `get_team_status` | - | 팀 현황 + 최근 이벤트 15개 |
| `get_project_overview` | - | 전체 Work Plan |
| `get_sync_status` | - | 중단 세션 감지 |
| `check_health` | product | 앱별 API 상태 |

## Work Plan 관리

| 도구 | 파라미터 | 설명 |
|------|---------|------|
| `add_work_plan` | appName, title, description, priority | 신규 등록 |
| `update_work_plan` | id, status, ... | 수정 |
| `list_work_plans` | appName, status | 필터 조회 |

## 문서 관리

| 도구 | 파라미터 | 설명 |
|------|---------|------|
| `upsert_document` | name, content, changedBy, ... | 생성/수정 |
| `get_document` | name | 조회 |
| `search_documents` | keyword | 검색 |
| `list_documents` | appName | 목록 |

## AI 설정

| 도구 | 파라미터 | 설명 |
|------|---------|------|
| `update_ai_config` | aiType, role, sectionKey, sectionValue | 섹션 저장 |
| `get_ai_config` | aiType, role | 전체 설정 조회 |
| `sync_all_ai_configs` | - | GitHub 동기화 |

## 팀 협업

| 도구 | 파라미터 | 설명 |
|------|---------|------|
| `update_my_status` | memberId, currentTask | 현재 작업 공유 |
| `session_start` | memberId, taskName | CC 세션 시작 |
| `session_end` | sessionId, commitHash, nextTask | CC 세션 완료 |
| `notify_task_complete` | appName, taskName, commitHash, nextTask | Slack 알림 |

## 페르소나

| 도구 | 파라미터 | 설명 |
|------|---------|------|
| `consult_persona` | role, question | 단일 관점 |
| `consult_all_personas` | question, focusRoles | 다중 관점 |
| `evolve_persona` | role, insight, source, impact | 지식 업데이트 |

## MCP 검증 방법

```bash
curl -X POST https://moon-server-production.up.railway.app/mcp \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -d '{"jsonrpc":"2.0","id":1,"method":"initialize","params":{"protocolVersion":"2024-11-05","capabilities":{},"clientInfo":{"name":"test","version":"1.0"}}}'
```
