# ADR-009: AI 설정 시스템 도입 (ai_configs 테이블)

- **날짜**: 2026-04-08
- **상태**: 승인
- **결정자**: developer_1

## 배경

Claude 등 AI 도구에 주입할 역할별 지침을 코드나 파일에 하드코딩하면,
채팅마다 재입력해야 하고 Gemini 등 다른 AI와 공유 불가.

## 결정

moon-server에 `ai_configs` 테이블을 도입해 AI별/역할별 설정을 DB에 저장.
MCP `update_ai_config` / `get_ai_config` 도구로 CRUD.

## 구조

```
ai_configs
  ai_type    (claude | gemini | ...)
  role       (all | developer_1 | developer_2 | planner)
  section_key (role_overview | cc_prompt_rules | ...)
  section_value (Markdown)
  version    (자동 증가)
```

## 해결한 버그

1. `metadata` JSONB 타입 불일치 → `@JdbcTypeCode(SqlTypes.JSON)` (583fb01)
2. McpHistoryAspect AOP outer tx 오염 → `REQUIRES_NEW` (a10d02b)

## 결과

- Claude 새 채팅 시작 시 `get_ai_config`로 역할 지침 즉시 로드 가능
- Gemini 등 다른 AI도 동일 설정 공유
