# MoonDeveloper — Planning Docs
> Last updated: 2026-04-08 | System: Git + ADR + MCP Index

## 기획서 관리 원칙
- 현재 최신 기획서 1개만 유지 (전체 버전 스냅샷 저장 안 함)
- Git이 이전 버전 복원 담당 (`git show <tag>:<path>`)
- ADR(decisions/)이 "왜 바꿨는지" 설명
- MCP moon-server가 AI 즉시 조회 인덱스

## 앱 목록
| 앱 | 폴더 | 버전 | 상태 |
|----|------|------|------|
| Splitly | apps/splitly/ | v1.0.5 | 🔴 ASC 심사 대기 |
| HabitFlow | apps/habitflow/ | v1.0.0 | 🟡 Store 제출 준비 |
| CoupleSync | apps/couplesync/ | v1.0.0 | 🟡 Store 제출 준비 |
| FocusOn | apps/focuson/ | v1.0.0 | 🟡 Store 제출 준비 |
| MoodFlow | apps/moodflow/ | v1.0.0 | 🟢 개발 진행중 |
| HomeTasks | apps/hometasks/ | v1.0.0 | 🟢 개발 진행중 |
| FreelancePay | apps/freelancepay/ | v1.0.0 | 🟢 개발 진행중 |
| PawLife | apps/pawlife/ | v1.0.0 | 🔵 기획 확정 |

## 버저닝
```bash
git tag -a docs/splitly-v1.0.5 -m "Splitly v1.0.5 기획서"
git show docs/splitly-v1.0.3:apps/splitly/overview.md
git diff docs/splitly-v1.0.3..docs/splitly-v1.0.5 -- apps/splitly/
```

## ADR 작성 규칙
decisions/ 폴더에 ADR-{번호}-{slug}.md 형식으로 저장
중요 결정 발생 시 MCP record_decision() 동시 기록
