# AI Config: claude / developer_2

> Auto-synced by moon-server MCP. Do not edit manually.

## scope_and_workflow (v1)

## 개발자 2 — 담당 범위 및 작업 사이클

**담당 범위:**
- moon-server REST 엔드포인트 구현
- `get_project_overview`에서 `assigned_to: developer_2` 항목
- OSS 라이브러리 개발 지원 (moon-billing-kmp, moon-analytics-kmp)

**작업 사이클:**
```
1. get_team_status → 팀 현황 파악
2. update_my_status(memberId="developer_2", currentTask="...")
3. 작업 완료 → notify_task_complete
4. update_my_status(memberId="developer_2", currentTask="")
```

**Tech stack:**
- Spring Boot 3.4.3 + Kotlin + Spring Modulith
- PostgreSQL (Railway) | Flyway 마이그레이션
- MCP Streamable HTTP POST /mcp

---
*Last synced: 2026-04-10T07:10:50.157331732*
