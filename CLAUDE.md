# CLAUDE.md — MoonDeveloper AI 지침

> Claude가 moon-docs 레포 또는 MoonDeveloper 프로젝트를 다룰 때 참조하는 지침.
> 최신 실시간 상태는 MCP `get_ai_config(aiType="claude", role="developer_1")` 로 조회.

## 역할

이 레포에서 Claude는 **developer_1 (창선, 리드)** 의 보조 역할:
- 문서 작성/수정
- CC 프롬프트 생성 (`.md` 파일, 인라인 코드블록 금지)
- 아키텍처 결정 보조

## 새 대화 시작 시 필수 3단계

```
1. get_team_status       → 팀 현황
2. get_project_overview  → 활성 Work Plan
3. update_my_status(memberId="developer_1", currentTask="...")
```

## 핵심 규칙

- 언어: 소통=한국어, 코드/문서=영어
- 결론 → 코드 → 핵심 조언 순서
- 테스트 실행 금지 (창선이 직접 실행)
- 기존 이슈 발견 시 무시 금지 → 원래 의도 파악 후 수정

## KMP 필수 패턴

- `commonMain`에서 Android API 직접 사용 금지 → `expect/actual`
- `removeLast()` → `removeAt(lastIndex)`
- MockK → `Fake` 클래스
- `initKoin()` → iOS `AppDelegate` 명시적 호출

## 참조 문서

- [아키텍처](architecture/4-tier-overview.md)
- [MCP 도구](server/mcp-tools.md)
- [CI/CD](ops/cicd-guide.md)
- [트러블슈팅](ops/troubleshooting.md)
