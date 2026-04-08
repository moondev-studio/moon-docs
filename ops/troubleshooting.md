# 공통 에러 & 해결법

## KMP

| 에러 | 원인 | 해결 |
|------|------|------|
| `removeLast()` 충돌 | Android 15 API | `removeAt(lastIndex)` |
| commonMain Firebase Auth 직접 호출 | KMP 제약 | `expect/actual` + AuthProvider Adapter |
| MockK KMP 컴파일 실패 | MockK 미지원 | `Fake` 클래스 |
| iOS 아이콘 App Store 거절 | alpha 채널 | `sips -s format bmp → sips -s format png` |
| iOS 앱 시작 시 DI 실패 | Koin 미초기화 | `AppDelegate`에서 `initKoin()` 명시 |

## moon-server

| 에러 | 원인 | 해결 |
|------|------|------|
| `metadata is of type jsonb but expression is of type varchar` | Entity JSONB 매핑 누락 | `@JdbcTypeCode(SqlTypes.JSON)` 추가 |
| `Transaction silently rolled back` | McpHistoryAspect outer tx 오염 | `HistoryService.recordAuto` → `@Transactional(REQUIRES_NEW)` |
| Flyway migration skipped | 프로덕션 체크섬 불일치 | Railway Data 탭 직접 SQL 실행 |
| Railway 배포 미트리거 | dev 브랜치만 push | `git checkout main && git merge dev && git push` |

## CI/CD

| 에러 | 원인 | 해결 |
|------|------|------|
| `ORG_GRADLE_PROJECT_*` 누락 | vanniktech 플러그인 필요 | `ORG_GRADLE_PROJECT_mavenCentralUsername/Password` |
| Maven Central publish 실패 | Gradle Configuration Cache | `--no-configuration-cache` 추가 |
| `secrets` in `if:` | GitHub Actions 제약 | `env` 컨텍스트로 래핑 |

## iOS 앱 심사

| 거절 사유 | 해결 |
|---------|------|
| PassKit 미사용 | 관련 코드/entitlement 제거 |
| ATT 팝업 누락 | `AppTrackingTransparency` 추가 |
| 계정 삭제 기능 없음 | 설정 > 계정 삭제 플로우 구현 |
| IAP 미구현 | StoreKit 연동 |
