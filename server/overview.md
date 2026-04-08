# moon-server 개요

## 스펙

- **Framework**: Spring Boot 3.4.3 + Kotlin + Spring Modulith
- **DB**: PostgreSQL (Railway) + Flyway 마이그레이션
- **배포**: Railway (branch=main, auto-deploy)
- **URL**: https://moon-server-production.up.railway.app

## API 경로

```
/api/splitly/v1/*
/api/habitflow/v1/*
/api/couplesync/v1/*
/api/focuson/v1/*
/api/vault/*          — CredentialVault
/mcp                  — MCP Streamable HTTP
```

## 핵심 테이블 (7개 필수)

| 테이블 | 용도 |
|--------|------|
| `work_plans` | 작업 추적 |
| `documents` | 프로젝트 문서 |
| `project_history` | 변경 이력 |
| `team_members` | 팀 멤버 |
| `sync_events` | MCP 동기화 이벤트 |
| `ai_configs` | AI 역할별 설정 |
| `chat_contexts` | Cross-AI 공유 메모리 |

## 배포 절차

```bash
git checkout main
git merge dev
git push origin main
# → Railway 자동 배포 트리거
# → Slack #moon-server 알림 확인
```

## Flyway 주의사항

- 프로덕션 migration 실패 시: Railway Postgres Data 탭 → 직접 SQL 실행
- `baseline-on-migrate`, `out-of-order`, `repair` 설정은 skipped migration 강제 실행 불가
- schema_version 직접 수정이 유일한 복구 방법
