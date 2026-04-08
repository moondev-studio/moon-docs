# MoonDeveloper — Project Documentation

> 4개 KMP 앱 + moon-server 운영 문서 허브
> 관리: Git + ADR + MCP 하이브리드
> 업데이트: moon-server MCP `sync_to_github_docs` 자동 동기화

## 빠른 링크

| 문서 | 설명 |
|------|------|
| [아키텍처 개요](architecture/4-tier-overview.md) | L1~L4 4-Tier 구조 |
| [MCP 도구](server/mcp-tools.md) | moon-server MCP 레퍼런스 |
| [CI/CD 가이드](ops/cicd-guide.md) | GitHub Actions + Railway |
| [트러블슈팅](ops/troubleshooting.md) | 공통 에러 & 해결법 |
| [OSS 가이드](oss/moon-kmp-libs.md) | Maven Central 배포 |
| [AI 지침](CLAUDE.md) | Claude 작업 지침 |

## 앱 현황

| 앱 | 버전 | Android | iOS | 설명 |
|-----|------|---------|-----|------|
| [Splitly](apps/splitly/store-status.md) | 1.0.3 | Internal | 심사중 | 지출 분할 |
| [HabitFlow](apps/habitflow/store-status.md) | 1.0.0 | 준비중 | 준비중 | 습관 추적 |
| [CoupleSync](apps/couplesync/store-status.md) | 1.0.0 | 준비중 | 준비중 | 커플 가계부 |
| [FocusOn](apps/focuson/store-status.md) | 1.0.0 | 준비중 | 준비중 | 집중 타이머 |
| MoodFlow | 1.0.0 | 개발중 | 개발중 | 감정 기록 |
| HomeTasks | 1.0.0 | 개발중 | 개발중 | 가사 분담 |
| FreelancePay | 1.0.0 | 개발중 | 개발중 | 프리랜서 정산 |
| PawLife | 1.0.0 | 기획 | 기획 | 반려동물 케어 |

## 기술 스택

- **KMP**: Kotlin 2.3.0 / Compose Multiplatform / AGP 8.13.2
- **Backend**: Spring Boot 3.4.3 + Railway + PostgreSQL
- **OSS**: `io.github.moondev-studio:*:1.0.0` (Maven Central)
- **CI/CD**: GitHub Actions (Android) + Xcode Cloud (iOS)
- **AI**: Claude MCP (moon-server 연동)

## 문서 관리 방식

1. **실시간 상태**: moon-server DB (MCP 도구로 조회)
2. **결정 사항**: `adr/` 및 `apps/*/decisions/` ADR 문서
3. **장기 보존**: 이 레포 (Git history)
4. **동기화**: `sync_to_github_docs` MCP 도구

## 버저닝
```bash
git tag -a docs/splitly-v1.0.5 -m "Splitly v1.0.5 기획서"
git show docs/splitly-v1.0.3:apps/splitly/overview.md
```
