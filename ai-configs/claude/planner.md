# AI Config: claude / planner

> Auto-synced by moon-server MCP. Do not edit manually.

## tools_and_product_status (v1)

## 기획자 — 핵심 도구 및 제품 현황

**핵심 도구:**
```
get_team_status       → 개발자들 현재 작업 확인
get_recent_changes    → 어제부터 무엇이 바뀌었나
consult_persona(role="pm", question="...") → PM 관점 제품 검토
```

**AdMob ID 확정 시 절차:**
```
upsert_document(
  name="AdMob-IDs-Confirmed",
  content="[HabitFlow]\nAndroid App ID: ca-app-pub-XXXX\n...",
  changedBy="planner",
  isPinned=True
)
```

**현재 제품 현황:**
| 앱 | 상태 | 다음 단계 |
|-----|------|---------|
| Splitly | 🔴 Apple 심사 대기 | 결과 대기 |
| HabitFlow | 🟡 제출 준비 | AdMob ID 확정 후 제출 |
| CoupleSync | 🟡 제출 준비 | Firebase Blaze + AdMob ID 후 제출 |
| FocusOn | 🟡 제출 준비 | AdMob ID 확정 후 제출 |

---
*Last synced: 2026-04-10T08:52:33.521051254*
