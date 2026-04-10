# AI Config: claude / all

> Auto-synced by moon-server MCP. Do not edit manually.

## architecture (v1)

## 아키텍처 빠른 참조

```
L1: moon-kmp-libs (OSS)     → io.github.moondev-studio:*:1.0.0
    moon-ui-kmp / moon-billing-kmp / moon-analytics-kmp
    moon-sync-kmp / moon-i18n-kmp / moon-auth-kmp

L2: moon-app-libs (Private) → includeBuild
    moon-server-kmp / moon-firebase-auth-kmp / moon-firestore-sync-kmp

L3: {app}/libs               → 앱별 비즈니스 로직
L4: {app}/composeApp         → UI

Backend: moon-server @ Railway
  /api/splitly/v1/*  /api/habitflow/v1/*
  /api/couplesync/v1/*  /api/focuson/v1/*
  /mcp  (Streamable HTTP)
```

**Base path**: `/Users/mcs/AndroidStudioProjects/MoonDeveloper/`

## communication_rules (v1)

## 커뮤니케이션 규칙
- 언어: 소통=한국어, 코드/변수명/MD=영어
- 결론 → 코드 → 핵심 조언 순서
- 불필요한 서론·반복 생략
- 기술 용어 원어 사용 (DI, MVI, expect/actual 등)

## mcp_tools (v1)

## MCP 도구 전체 목록

### 📋 상태 조회
| 도구 | 용도 |
|------|------|
| `get_team_status` | 팀 현황 + 최근 이벤트 |
| `get_project_overview` | 전체 Work Plan |
| `get_recent_changes(sinceHours=24)` | N시간 내 변경사항 |
| `get_sync_status` | MCP 동기화 상태·중단 세션 감지 |
| `check_health` | moon-server 헬스 체크 |

### 📝 Work Plan 관리
| 도구 | 용도 |
|------|------|
| `add_work_plan(appName, title, description, priority)` | 새 작업 등록 |
| `update_work_plan(id, status, title, description, priority)` | 작업 상태 변경 |
| `list_work_plans(appName, status)` | 조건부 조회 |

### 📄 문서 관리
| 도구 | 용도 |
|------|------|
| `upsert_document(name, content, changedBy, roleAccess, isPinned)` | 문서 생성/수정 (버저닝 자동) |
| `get_document(name)` | 문서 조회 |
| `search_documents(keyword)` | 키워드 검색 |
| `list_documents(appName)` | 전체 목록 |

### 🤝 팀 협업
| 도구 | 용도 |
|------|------|
| `update_my_status(memberId, currentTask)` | 현재 작업 공유 |
| `session_start(memberId, taskName)` | CC 세션 시작 기록 |
| `session_end(sessionId, commitHash, nextTask)` | CC 세션 완료 기록 |

### 🎭 페르소나 시스템
| 도구 | 용도 |
|------|------|
| `list_personas()` | 6개 페르소나 목록 |
| `consult_persona(role, question)` | 단일 관점 |
| `consult_all_personas(question, focusRoles)` | 다중 관점 분석 |
| `evolve_persona(role, insight, source, impact)` | 페르소나 지식 진화 |

### 🔔 알림
| 도구 | 용도 |
|------|------|
| `notify_task_complete(appName, taskName, commitHash, nextTask)` | CC 완료 알림 |
| `send_billing_alert` | 비용 알림 |

## persona_system (v1)

## 페르소나 시스템 사용법

6개 역할의 AI 페르소나가 moon-server DB에 저장되어 있습니다.

**역할 목록:**
```
🗂️ pm          — 로드맵·모듈 재사용·수익 우선순위
⚙️ be_dev       — moon-server·Railway·API 설계
📱 app_dev      — KMP·Compose·iOS/Android 호환
🌐 frontend_dev — 웹·Firebase Hosting·딥링크
🔍 qa           — 테스트·릴리즈 체크·크래시 패턴
🎨 designer     — MoonTheme·Gemini 요청·접근성
```

**언제 어떤 도구를 쓰나:**
```
중요 기술 결정  → consult_all_personas(question="...", focusRoles="pm,app_dev,be_dev")
서버 설계 고민  → consult_persona(role="be_dev", question="...")
KMP 구현 고민  → consult_persona(role="app_dev", question="...")
릴리즈 전 체크  → consult_persona(role="qa", question="이 버전 릴리즈 준비됐나?")
UI 방향 결정   → consult_persona(role="designer", question="...")
작업 우선순위  → consult_persona(role="pm", question="...")
```

**좋은 아이디어·교훈 발견 시 페르소나 진화:**
```
evolve_persona(
  role="app_dev",
  insight="KMP에서 JSONB 직접 사용 시 Room Entity와 타입 충돌 발생 → 별도 DTO 레이어 필수",
  source="developer_1",
  impact="high"
)
```
→ Slack #moon-dev-alerts 자동 알림 + 다음 consult부터 이 교훈 포함

## role_overview (v1)

# MoonDeveloper — Project Instructions (v4)
> 갱신: 2026-04-02 | 역할: 개발자 1·2 + 기획자 공통 사용

## 역할 확인

| 역할 | memberId | 담당 |
|------|----------|------|
| **개발자 1** (창선, 리드) | `developer_1` | 전체 아키텍처·KMP·CI/CD·OSS |
| **개발자 2** | `developer_2` | moon-server REST·OSS 개발 지원 |
| **기획자** | `planner` | 로드맵·AdMob ID·제품 방향·디자인 |

## session_start_rules (v1)

## 새 채팅 시작 (3단계 필수)

```
1. get_team_status          → 팀 현황 + 최근 변경사항
2. get_project_overview     → 활성 Work Plan 전체
3. update_my_status(memberId="[본인 역할]", currentTask="[지금 할 일]")
```

> 채팅 종료 시: `update_my_status(memberId="...", currentTask="")` 로 상태 초기화

---
*Last synced: 2026-04-10T08:52:29.957896095*
