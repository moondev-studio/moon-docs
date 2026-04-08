# moon-kmp-libs OSS 가이드

## Maven Central 좌표

```kotlin
implementation("io.github.moondev-studio:moon-ui-kmp:1.0.0")
implementation("io.github.moondev-studio:moon-billing-kmp:1.0.0")
implementation("io.github.moondev-studio:moon-analytics-kmp:1.0.0")
implementation("io.github.moondev-studio:moon-sync-kmp:1.0.0")
implementation("io.github.moondev-studio:moon-i18n-kmp:1.0.0")
implementation("io.github.moondev-studio:moon-auth-kmp:1.0.0")
implementation("io.github.moondev-studio:moon-ocr-kmp:1.0.0")
```

## 배포 정보

- **Group**: `io.github.moondev-studio`
- **Namespace**: `bbaxiembvn` (verified)
- **GPG Key**: A5B5E603 (keyserver.ubuntu.com)
- **현재 버전**: 1.0.0

## 배포 방법

```bash
./gradlew publishAllPublicationsToMavenCentralRepository \
  --no-configuration-cache
```

## GitHub Actions 필수 Secrets

```
ORG_GRADLE_PROJECT_mavenCentralUsername
ORG_GRADLE_PROJECT_mavenCentralPassword
ORG_GRADLE_PROJECT_signingInMemoryKey
ORG_GRADLE_PROJECT_signingInMemoryKeyId
ORG_GRADLE_PROJECT_signingInMemoryKeyPassword
```

## CoupleSync 주의사항

CoupleSync는 `includeBuild` 제거 시 R8 duplicate class 에러 발생.
→ dual-substitution 패턴 유지 필수 (OSS 전환 완료 후에도).
