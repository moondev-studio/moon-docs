# AI Config: claude / developer_1

> Auto-synced by moon-server MCP. Do not edit manually.

## architecture_rules (v1)

## 개발자 1 — Architecture Rules (KMP)
- commonMain: Android 전용 API 직접 사용 금지 → `expect/actual`
- `removeLast()` → `removeAt(lastIndex)` (Android 15 호환)
- MockK in KMP → `Fake` 클래스
- iOS 아이콘 alpha → sips BMP roundtrip 제거
- `initKoin()` → iOS AppDelegate 명시적 호출

## cc_prompt_rules (v1)

## 개발자 1 — CC 프롬프트 규칙
- 모든 CC 프롬프트 → `.md` 파일 생성 (인라인 코드블록 금지)
- CC 세션 시작: `session_start(memberId="developer_1", taskName="...")`
- CC 세션 종료: `session_end(sessionId, commitHash, nextTask)` + `notify_task_complete`
- 테스트 실행 금지 — Android Studio 터미널에서 직접 실행
- 발견된 기존 이슈 → 무시 금지, 원래 의도 파악 후 수정

## credentials (v1)

## 개발자 1 — Key Credentials (CredentialVault에서 fetch)
- Apple Team ID: `667PKR9RY6` | Bundle: `com.moondeveloper`
- ASC Key: `29JY53H377` (만료 2026-09-27)
- iOS p12 password: `vega6732` (만료 2027-03-31)
- Sandbox: `moonDeveloper@tester.com` / `Vega6732@@` / 대한민국
- Vault API: `GET /api/vault/credential` + `VAULT_BUILD_TOKEN`

---
*Last synced: 2026-04-10T07:10:49.123976003*
