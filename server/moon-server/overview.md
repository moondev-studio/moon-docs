# moon-server — Overview
> Version: 2.0 | Updated: 2026-04-08

## 배포
- Platform: Railway (auto-deploy: dev → main push)
- URL: https://moon-server-production.up.railway.app
- DB: PostgreSQL (Railway managed)

## 구조
- Spring Boot 3.4.3 + Kotlin + Spring Modulith
- Flyway 마이그레이션
- MCP Streamable HTTP POST /mcp

## MCP 도구 (~60개)
- 상태 조회: get_team_status, get_project_overview, check_health
- Work Plan: add/update/list_work_plans
- 문서: upsert_document, get_document, search_documents
- 팀: update_my_status, session_start/end
- 히스토리: record_decision, create_history_entry, search_history
- 페르소나: list_personas, consult_persona, evolve_persona
- CredentialVault: get/set/delete_credential
- 알림: notify_task_complete, send_billing_alert

## 앱별 API 엔드포인트
- /api/splitly/v1/*
- /api/habitflow/v1/* (구현 예정)
- /api/couplesync/v1/* (구현 예정)
- /api/focuson/v1/* (구현 예정)

## CredentialVault
- 암호화: AES-256-GCM
- 테이블: vault_credentials
- 21개 크리덴셜 등록
- API: GET /api/vault/credential (Bearer token)

## 관련 Work Plan
- [6] WebSocket Implementation (FocusOn 스터디룸)
- [50] Slack 채널 자동 생성
- Session D: 채팅 컨텍스트 저장 도구 추가 (예정)
